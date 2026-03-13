MyTween

MyTween is a lightweight tweening system built primarily as a learning project to understand how tweening architectures work internally.

This is not intended to compete with professional tweening libraries. Instead, it focuses on exploring design ideas, performance considerations, and low-allocation data handling.

The system is intentionally minimal and is best used for educational purposes or simple animations such as UI transitions.

Design Goals

Learn how tweening systems are structured internally.

Reduce garbage collection pressure during runtime.

Keep data structures compact and cache-friendly.

Avoid unnecessary allocations such as closures where possible.

Provide a simple but functional tween workflow.

Core Architecture

At the heart of the system is the TweenData struct.

Struct-Based Tween Storage

Tween information is stored inside a struct instead of a class. This helps reduce heap allocations and lowers garbage collection pressure.

TweenData contains only the minimal data needed for tween execution, including a single delegate.

Callback Organization

To keep TweenData small and efficient, callbacks such as:

OnUpdate

OnFinish

OnKill

are grouped and stored separately in collections inside TweenManager.

This design keeps the core tween struct lightweight and avoids bloating the data layout.

Object Pooling

Tween instances are reused through object pooling.

The pool does not grow dynamically.
To change the number of available tweens, modify:

MAX_TWEEN

in TweenManager.cs.

Allocation Avoidance

Closures are a common hidden source of garbage allocations in tween systems.

To reduce this:

The tween creation API supports generics:

TweenCreate<TTarget, TValue>

TTarget – the object being modified

TValue – the value used during interpolation

This allows updating targets via OnUpdate without capturing variables inside closures in many cases.

Easing System

The system includes 30 easing functions located in:

EasingMagic.cs

A switch expression selects the appropriate easing function during tween evaluation.

Time Handling
Unscaled Time Support

Tweens can run using unscaled time, allowing animations to continue when the game is paused.

Features

Struct-based tween data

Object pooling

Unscaled time support

Generic tween creation to reduce closure allocations

30 easing functions

Basic tween lifecycle callbacks

Available callbacks:

OnUpdate()

OnFinish()

OnKill()

Tween control methods:

Kill()

Kill(float afterTime)

KillAll()

PingPong

Ping-pong behaviour can be enabled using:

PingPong(int loopCount)

Behavior rules:

-1 → Loop forever

> 0 → Loop until the count decrements to zero

0 → Ping-pong disabled

Tween Chaining

Basic chaining is available through:

ThenDO()

However, this feature is limited and not recommended for heavy usage.

For example:

UnscaledTime may not propagate correctly through chained tweens.

Use chaining only if you understand the underlying behavior.

Intended Use

This system is suitable for:

Learning tween architecture

Simple UI animations

Small projects where minimal allocations are desired

It is not designed as a production-ready replacement for full tweening libraries.
