[link Github](https://gist.github.com/ksvbka/ecbbdab3cd3c4913ac43)
[link giải thích](https://www.codeproject.com/Articles/1087619/State-Machine-Design-in-Cplusplus-2)


```c
#define MAX_BUTTON_COUNT    10      // can handle max 10 button, can adjust to meet requirement
#define CLICK_TICK          200	 // 200ms
#define DOUBLE_CLICK_TICK   200	 // 200ms
#define HOLD_TICK           500     // 500ms

#define SYSTEMCALLBACK VOID (*fnCallback) (*void);

typedef enum tagSTATE
{
    IDLE,
    WAIT_BUTTON_UP,
    WAIT_PRESS_TIMEOUT,
    WAIT_CLICK_TIMEOUT,
    WAIT_DCLICK_TIMEOUT
}state;


typedef struct tagButton
{
    WORD 	nTick;		// Current tick of button.
    WORD 	gpioPin;		// GPIO PIN connect witch button.
    BOOL 	enable;		// Enable or disable button.
    BYTE	state;			// State of button.
    SYSTEMCALLBACK  fnCallbackIDLE;	// Function for handle IDLE event
    SYSTEMCALLBACK  fnCallbackClick;	// Function for handle Click event
    SYSTEMCALLBACK  fnCallbackDoubleClick;	// Function for handle DoubleClick event    
    SYSTEMCALLBACK  fnCallbackHold;	// Function for handle Hold event    
 
}BUTTON, *PBUTTON; // Define Button object and pointer to Button.
```

Lưu ý, do ngôn ngữ C không hỗ trợ hướng đối tượng nên các phương thức sẽ được khai báo bằng các con trỏ hàm. Như ở ví dụ trên, SYSTEMCALLBACK là một macro định nghĩa con trỏ


```c

// Array store pointer to button
INTERNAL PBUTTON g_pButtons[MAX_BUTTON_COUNT] = {{NULL}};

```

Để xử lý được nhiều button, ta sẽ định nghĩa ra một mảng chứa các con trỏ hàm, mỗi phần tử trong hàm này sẽ trỏ đến một đối tượng Button.
Khi muốn sử dụng button, ta chỉ việc định nghĩa Button và gán vào một vị trí trong mảng trên. Mỗi vòng lặp, hệ thống sẽ thực hiện quét các phần tử trong mảng và lần lượt xác định trạng thái các button và gọi hàm callback tương ứng


```c

/*-----------------------------------------------------------------------------*/
/* Function prototypes  */
/*-----------------------------------------------------------------------------*/
 
/*
    initialization Button and regist callback function to handle event
*/
VOID ButtonInit(PBUTTON button, WORD GPIO_PIN, SYSTEMCALLBACK onIDLE, SYSTEMCALLBACK onClick, SYSTEMCALLBACK onDoubleClick, SYSTEMCALLBACK onHold);
 
/*
    Enable Button, regist to array g_pButtons[]and start listion event, recall while button cancel;
*/
BOOL ButtonStart(PBUTTON pBtnRegister);
 
/*
    Detroy Button, delete from g_pButtons[].
*/
VOID ButtonCancel(BUTTON btnCancel);
 
/*
Sweep g_pButtons[] and check state of each button and call the callback function to handle event.
*/
VOID ButtonProcessEvent(VOID);
```


Ở mỗi vòng lặp, hệ thống sẽ quét mảng g_pButtons để xác định các phần tử mảng đã được gán với Button chưa, đồng thời, các button này có được enable hay không. Nếu các điều kiện này thỏa mãn sẽ thực hiện switch trạng thái Button.


```c
if(g_pButtons[nIndex] != NULL && g_pButtons[nIndex]->enable == 1){
    switch(g_pButtons[nIndex]->state){
        case IDLE:
		    // Waiting for One pin being pressed.
			if(BUTTON_LOGIC_LEVEL(nIndex) == LOW){
				g_pButtons[nIndex]->state = WAIT_BUTTON_UP;
				g_pButtons[nIndex]->nTick = CURRENT_TICK + HOLD_TICK;
			}
	    break;
	    case WAIT_BUTTON_UP:
		    // waiting for One pin being released.
		    if(BUTTON_LOGIC_LEVEL(nIndex) == HIGH && g_pButtons[nIndex]->nTick > CURRENT_TICK){
	            g_pButtons[nIndex]->state = WAIT_CLICK_TIMEOUT;
	            g_pButtons[nIndex]->nTick = CURRENT_TICK + CLICK_TICK;
			}	    
            break;
      }
      // Detect hold event.
      if(BUTTON_LOGIC_LEVEL(nIndex) == HIGH && g_pButtons[nIndex]->nTick <= CURRENT_TICK)
      {
            if(g_pButtons[nIndex]->fnCallbackHold != NULL)
            {
                  g_pButtons[nIndex]->fnCallbackHold(NULL); // Process hold event.
                  __delay_cycles(500000);
            }
            g_pButtons[nIndex]->state = IDLE;
            g_pButtons[nIndex]->nTick = 0;
            break;
      }
 break;
	    
	}
}
```


