
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
>Transform.hpp
```cpp
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

>Collision.hpp
```cpp
// This struct is likely used to represent the result of  collision between two objects, 
// providing detailed information about the collision event.
struct Collision{

CollisionBody* bodyA{};
CollisionBody* bodyB{};
Manifold manifold;
};

```

```cpp
struct Manifold
{

	Manifold(const Vector2& a, const Vector2& b, const Vector2& normal, float depth);
	
	Manifold(const Vector2& normal, float depth);
	
	Manifold();
	
	Vector2 a;
	Vector2 b;
	Vector2 normal;
	float depth{};
	
	bool hasCollision{};
	
	static Manifold Empty(){
	return {};
	}
	
	[[nodiscard]] Manifold Swaped() const;
	
	};
	
	std::ostream& operator<<(std::ostream& os, const Manifold& manifold);

}
```


# Collider

`TestCollision()` is a function which can take argument overload for any other type of shape collider
>Collider.hpp
```cpp
class Collider
{
  public:
    Collider() = default;
    Collider(Collider &&col) noexcept = default;
    virtual ~Collider() = default;
    Collider &operator=(Collider &&col) = default;
    Collider &operator=(const Collider &col) = default;
    Collider(const Collider &col) = default;

    /**
     * \brief The center of the collider.
     */
    Vector2 center{};

    virtual Manifold TestCollision(const Transform *transform, const Collider *collider,
                                   const Transform *colliderTransform) const = 0;

    virtual Manifold TestCollision(const Transform *transform, const BoxCollider *collider,
                                   const Transform *boxTransform) const = 0;


    virtual Manifold TestCollision(const Transform *transform, const CircleCollider *collider,
                                   const Transform *circleTransform) const = 0;


    virtual Manifold TestCollision(const Transform *transform, const AabbCollider *collider,
                                   const Transform *aabbTransform) const = 0;


    [[nodiscard]] virtual Vector2 FindFurthestPoint(const Transform *transform, const Vector2 &direction) const = 0;

    /**
     * \brief Gets the size of the box that surrounds the collider.
     * \return The bounding box of the collider.
     */
    [[nodiscard]] virtual Vector2 GetBoundingBoxSize() const = 0;
};
```
