
# Overview

The `WindowClass` is essential for creating and managing application windows.

## 1. **Window Creation**

- **Namespace Setup**: namespace `Chil::win` Create a dedicated folder for your window-related code. This helps in organizing your project.
- **Wrapper for `windows.h`**: Instead of directly including `windows.h`, create a wrapper (e.g., `chillwin.h`) to specify the version of Windows and disable unnecessary features


1. These message handlers are responsible for:
    
    - Setting up the window's user data to store a pointer to the IWindow object.
    - Changing the window procedure to use HandleMessageThunk_ after initial setup.
    - Forwarding messages to the appropriate IWindow object's HandleMessage_ function.
2. The ForwardMessage_ static function in IWindowClass allows derived classes to forward messages to the IWindow's HandleMessage_ function.
    

This design allows for a clean separation between the window class registration process and the actual window instances, while providing a mechanism to route Windows messages to the appropriate object-oriented handlers.

### Try again with different contex