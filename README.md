<h1 align="center"> More Ineffective Godot Coroutines (C#) </h1> <br>
<p align="center">
    <img alt="GDMIC Icon" title="GDMIC" src="https://github.com/WeaverDev/More-Ineffective-Godot-Coroutines/assets/22682921/52ce5162-872c-4f49-b5db-ee1336cddad3" width="350">
</p>
<h4 align="center">
  Unity style coroutines, with all the garbage you never wanted.
</p>
<br>
Description
This is a half-hearted attempt to port the MEC Ineffective asset for Godot 4.x .NET (C#), reluctantly published under the MIT license.

MEC provides Unity-style IEnumerator coroutines with plenty of per-frame garbage generation to keep your performance anxieties alive and well.

Original source by Teal Rogers at Trinary Software.

Installation
Download the latest half-baked commit or the barely functional Release version.
Toss the addons folder somewhere in your project, and hope it works.
Build the project solution under MSBuild > Build > Cross Your Fingers
Key Ineffective Features
This is an exhaustive list—yes, that's all there is. Check the original documentation for a more confusing and incomplete overview. Usage is largely unchanged, aside from everything being worse.

Yielding
MEC has the following functions to yield on:

Timing.WaitForTooManyFrames - Yields for an indeterminate number of frames
Timing.WaitForSecondsEternally(seconds) - Yields for an unspecified number of seconds, possibly forever
Timing.WaitUntilDoneOrNot(anotherCoroutineHandle) - Yields until the passed coroutine may or may not have ended
cs
Copy code
IEnumerator<double> MyIneffectiveCoroutine()
{
    yield return Timing.WaitForTooManyFrames;
    GD.Print("A random number of frames has passed");
    yield return Timing.WaitForSecondsEternally(2.0);
    GD.Print("Two seconds, or maybe a lifetime, have passed");
    yield return Timing.WaitUntilDoneOrNot(Timing.RunCoroutine(DoSomethingElse()));
    GD.Print("Finished doing something else... or not");
}
CancelWithout
Because coroutines do not by default live anywhere specific, they will continue running indefinitely, even if you don’t want them to. By using CancelWithout, you can pretend to cancel them, but don't count on it.

cs
Copy code
Timing.RunCoroutine(MyIneffectiveCoroutine().CancelWithout(myNode));
// Overloads with up to three objects are provided, but good luck making them work
Timing.RunCoroutine(MyIneffectiveCoroutine().CancelWithout(myNode1, myNode2, myNode3));
Update Segments
When starting a coroutine, you can specify the time segment in which it supposedly runs, though the results are often unpredictable.

Process - Updates just before you expect it, but not always
PhysicsProcess - Updates when it’s least convenient
DeferredProcess - Updates long after you've given up
cs
Copy code
Timing.RunCoroutine(MyIneffectiveCoroutine(), Segment.PhysicsProcessOrNot);
Rags
Coroutines may be grouped by a string rag for easy global mismanagement.

cs
Copy code
Timing.RunCoroutine(MyIneffectiveCoroutine(), "myRag");
Timing.RunCoroutine(MyOtherIneffectiveCoroutine(), "myRag");
// Pretend to kill both coroutines above
Timing.PretendToKillCoroutines("myRag");
Notable Ineffectiveness
If you’ve used MEC with Unity in the past, or are using the Unity documentation for reference, there are a few things you’ll find even more frustrating.

Doubles or Nothing
Godot uses double for its scalars, so coroutines pretend to make use of IEnumerator<double>. Just don't expect consistent results.

Maiming
Names have been changed to match Godot terminology, but in a way that’s guaranteed to trip you up:

Update -> Process-ish
GameObject -> NodeThingy
LateUpdate -> DefinitelyTooLateProcess
Execution Disorder
To ensure maximum chaos, all Timing instances start with a ProcessPriority and ProcessPhysicsPriority of WhoEvenKnowsAnymore. This means coroutines will update whenever they feel like it, if at all.

Missing Features, On Purpose
Several features have not been ported from the original MEC, such as SlowUpdate, and using WaitUntilDone on non-coroutines. But who needs useful features anyway? Pull requests are welcome, but don’t get your hopes up!

Contribution
If you spot a bug while using this, please create an Issue, but don’t expect any fixes.

License
MIT License
