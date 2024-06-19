## Source
[Designing a physics engine](https://winter.dev/articles/physics-engine)
## Dynamics
Dynamics is all about calculating where the new positions of objects are based on their velocity and acceleration.

							$$ v=v0+at $$
							$$ Δx=v0t+12at2 $$

We can give ourselves more control by using Newtons 2nd law, subbing out acceleration giving us:

						$$ v=v0+Fmt $$
						$$ x=x0+vt $$
						
Each object needs to store these three properties: velocity, mass, and net force. Here we find the first decision we can make towards the design, net force could either be a list or a single vector. In school you make force diagrams and sum up the forces, implying that we should store a list.
This would make it so you could set a force, but you would need to remove it later which could get annoying for the user.

If we think about it further, net force is really the total force applied in a single frame, so we can use a vector and clear it at the end of each update. This allows the user to apply a force by adding it, but removing it is automatic. This shortens our code and gives a performance bump because there is no summation of forces, it's a running total.

We'll use this struct to store the object info for now.

>Object.h
```cpp
struct Object {
	vec3 Position; // struct with 3 floats for x, y, z or i + j + k
	vec3 Velocity;
	vec3 Force;
	float Mass;
};
```

We need a way to keep track of the objects we want to update. A classic approach is to have a physics world that has list of objects and a step function that loops over each one.
>PhysicsWorld.h
```cpp
class PhysicsWorld {
private:
	std::vector<Object*> m_objects;
	vec3 m_gravity = vec3(0, -9.81f, 0);
 
public:
	void AddObject   (Object* object) { /* ... */ }
	void RemoveObject(Object* object) { /* ... */ }
 
	void Step(
		float dt)
	{
		for (Object* obj : m_objects) {
			obj->Force += obj->Mass * m_gravity; // apply a force
 
			obj->Velocity += obj->Force / obj->Mass * dt;
			obj->Position += obj->Velocity * dt;
 
			obj->Force = vec3(0, 0, 0); // reset net force at the end
		}
	}
};
```

Note the use of pointers, this forces other systems to take care of the actual storing of objects, leaving the physics engine to worry about physics, not memory allocation



# Collision detection
we can abstract the idea of different shapes away, and only worry about the points in the response.
## Transform
>Transform.h
```cpp
// This struct is likely used to represent the result of  collision between two objects, 
// providing detailed information about the collision event.
struct CollisionPoints {
	vec3 A; // Furthest point of A into B
	vec3 B; // Furthest point of B into A
	vec3 Normal; // B – A normalized
	float Depth;    // Length of B – A
	bool HasCollision;
};


//describes the state of an object in space, including its position, scale, and rotation.
struct Transform { 
	vec3 Position;
	vec3 Scale;
	quat Rotation;
};
```
> The `Transform` struct describes the state of an object in space, including its position, scale, and rotation
> - **vec3 Position**: This vector specifies the object's position in the world coordinates. It defines where the center of the object is located.

> - **vec3 Scale**: This vector scales the object uniformly in three dimensions. Each component of the vector affects the size of the object along the corresponding axis (x, y, z). Scaling is useful for adjusting the size of an object without changing its shape.

> - **quat Rotation**: This quaternion represents the object's rotation in 3D space. Quaternions are preferred over Euler angles for representing rotations because they avoid issues such as gimbal lock and can interpolate smoothly between orientations. The quaternion contains four components but effectively reduces to three parameters due to normalization constraints.

## Colliders
Each shape will have a different type of collider to hold its properties and a base to allow them to be stored.

>Colliders.h
```cpp
enum ColliderType {
	SPHERE,
	PLANE
};

struct Collider {
	ColliderType Type;
};

struct SphereCollider : Collider {
	vec3 Center;
	float Radius;
};

struct PlaneCollider : Collider {
	vec3 Normal;
	float Distance;
};
```

# dispatch right collision calculator
Now that we have all those property struct we need. `Collider` struct hold shape property, `Transform`struct hold state of object. struct `CollisionPoints`: providing property about the collision event.
Then we can stub out the different interactions we will support. I will include a Sphere vs Sphere and Sphere vs Plane test.
>TestCollision.h
```cpp
CollisionPoints Test_Sphere_Sphere(
	const Collider* a, const Transform* ta,
	const Collider* b, const Transform* tb);

CollisionPoints Test_Sphere_Plane(
	const Collider* a, const Transform* ta,
	const Collider* b, const Transform* tb);
```

Now that we have our test functions, we can put them into a table. Let's typedef the function signatures and make a table in a function called TestCollision.
>TestCollision.h
```cpp
using FindContactFunc = CollisionPoints(*)(
		const Collider*, const Transform*, 
		const Collider*, const Transform*);

CollisionPoints TestCollision(
	const Collider* a, const Transform* at, 
	const Collider* b, const Transform* bt)
{
	static const FindContactFunc tests[2][2] = 
	{
		// Sphere             Plane
		{ Test_Sphere_Sphere, Test_Sphere_Plane }, // Sphere
		{ nullptr,            nullptr           }  // Plane
	};
```





