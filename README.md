# My-Tween

-I Wanted to learn how the Tweening system architecture operates and work, so I made my own tweening system/library.
-this is not a full fledged Library like the one's professionals make and sell. instead is just the bare bones version I made from how far I can think in terms of design and use cases.
- I have made this system by keeping performnace and speed in my mind so to avoid so many garabage collection and CPU jumping between requests, struct is used to store pure data in TweenData, just one delegate is used inside this struct.
----------------------------------------------------------------------------
What It offers:-
-Unscaled Time Execution is supported in case of game pause.
- This Tween system is pretty good for only simple tweening of stuff, like UI animation etc. chaining of tween is available in a very limited not recommeneded to use kind of way until and unless you understand or desire that behaviour example the UnscaledTime doesn't work for all Tween that are chained using ThenDO(). so if you are going to use this, use for eduational purpose of learning about how a tween system at a very beginner stage looks like and work.
- At its core the system is built upon struct TweenData so Garbage collection is none or very little if any. 
- Object Pooling is used for optimization, and it doesn't grow dynamically so change MAX_TWEEN value in TweenManager.cs for your use requirements.
- struct based Tween Data.
- callbacks are grouped and kept in collection based on category in Tween Manager to keep the struct Tween datatype small.
- also to avoid closures in Tween Helper methods for various use cases if you want to make your own I have made TweenCreation method generic to allow <TTarget, TValue> here TValue is the value to be used in lerp for start and end. TTarget is if you want to update the actual object you are applying tween on so you can use OnUpdate action to modify target without generating closures and capture variables to some extent. you can look inside the code in Tween.cs to understand this.
- There are 30 easing functions in the EasingMagic.cs file. and to choose one of them, switch expression is used to evaluate the appropriate function during update while lerping.
- Features OnUpdate(), OnFinish(), OnKill(), Kill(), Kill(float afterTime),KillAll(), PingPong()[-1=>Loop Forever, value > 0 will loop till it decrements and reaches zero, 0 => ping pong is not active this means.]
