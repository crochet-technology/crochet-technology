# About  [**Chochet-technology**]

[![](https://camo.githubusercontent.com/666bcaed9816ac45e5d2a0f5595b2de6b2e38eb1fa3e8c54902e156224103545/68747470733a2f2f6b6f6d617265762e636f6d2f67687076632f3f757365726e616d653d6e69686172696b6132383130)](https://camo.githubusercontent.com/666bcaed9816ac45e5d2a0f5595b2de6b2e38eb1fa3e8c54902e156224103545/68747470733a2f2f6b6f6d617265762e636f6d2f67687076632f3f757365726e616d653d6e69686172696b6132383130)

Note: I am available  here for more updates.

In **chochet-technology**  Google Developer Expert team for Android and currently work as  software making.

# ToastHandler

Reference from  [StackOverFlow](https://stackoverflow.com/questions/51532449/fatal-exception-android-view-windowmanagerbadtokenexception-unable-to-add-wind)

An Android library wrapper over  [ToastCompat](https://github.com/PureWriter/ToastCompat)  for handling Toast BadTokenException happening on Android API level 25 and showing Toast smoothly on All Android versions. (Handling some memory leaks and changed to Kotlin with Custom Lint added)



### [](https://github.com/niharika2810/ToastHandler#usage)Usage

Add this to your app build.gradle:

implementation "com.toastfix:toastcompatwrapper:1.2.0"

Use this wherever you are showing Toast:

Java

ToastHandler.showToast(this, "Hello,I am Toast", Toast.LENGTH_SHORT);

Kotlin

ToastHandler.showToast(this, "Hello,I am Toast", Toast.LENGTH_SHORT)

  
Also, If you forget to use it, Let the Android Lint help you :-)

Latest Feature:

Added a custom lint which disallow the usage of Android's  [Toast](https://developer.android.com/reference/android/widget/Toast)  class in favor of  `ToastHandler`.

These are the properties of the custom lint.

message          = "Usage of android Toast is prohibited"
briefDescription = "The android Toast should not be used"
explanation      = "The android Toast should not be used, use ToastHandler instead to avoid BadTokenException on Android API level 25"
category         = Category.CORRECTNESS
priority         = 6
severity         = Severity.WARNING

The custom lint is added in the  `toasthandler`  module as  `lintPublish`. So, adding the library dependency into any project will also include this custom lint.

This lint replaces  `Toast.makeText`  with  `ToastHandler.getToastInstance`.

- Toast.makeText(this, "Hi, I am Toast", Toast.LENGTH_SHORT).show()
+ ToastHandler.getToastInstance(this, "Hi, I am Toast", Toast.LENGTH_SHORT).show()

Here's a small GIF showing how this will look in the IDE.

[![lint](https://user-images.githubusercontent.com/22273871/94834413-fa0b0e00-042d-11eb-8dda-26087568176e.gif)](https://user-images.githubusercontent.com/22273871/94834413-fa0b0e00-042d-11eb-8dda-26087568176e.gif)

# Android Data Binding: Under the Hood (Part 1)

Every time, we have to refer the views from these layouts, we use “[**findViewById**](https://developer.android.com/reference/android/app/Activity#findViewById(int))” in our Activity/Fragment. But doing this for a large number of views, create a lot of boilerplate code and there is always a risk of a null pointer exception due to an invalid view ID.

With technology advancements, Android has given us very cool alternatives to be adopted and get rid of these above-mentioned limitations with some performance improvements. But to adopt something for our code or not, we should know how exactly it is implemented:

**a) Kotlin Android Extensions:** Plugin that will allow you to access views in the layout XML, just as if they were properties with the name of the id you used in the layout definition.

Add this in your app  **build.gradle**:

plugins **{** id 'kotlin-android-extensions' **}**

Now you can reference the view ids with the same name they are in your layout files.

**Under the hood:**

> The plugin will generate some extra code (Tools –> Kotlin, called Show Kotlin Bytecode -> Decompile) that **will allow you to access views in the layout XML, just as if they were properties**  with the name of the id you used in the layout definition. It builds a local view cache. So the first time a property is used, it will do a regular  _findViewById_. But next time, the view will be recovered from the cache, so the access will be faster. Check more  [here](https://antonioleiva.com/kotlin-android-extensions/).

Well, there are some known issues while using them:

-   They are  **Kotlin**  only. :-(
-   Synthetic properties  **don’t work cross-module**. Check this  [open issue](https://youtrack.jetbrains.com/issue/KT-22430).
-   There are  **no checks**  against invalid lookups. We won’t get any error if the id, we just referenced in our class, is not actually there in our layout due to synthetic properties exposing a global namespace of ids which are unrelated to the layout.

**b) View Binding:** It  generates a  _binding class_  for each XML layout file present in that module. An instance of a binding class contains direct references to all views that have an ID in the corresponding layout.

Add this in your app  **build.gradle**:

android {  
    ...  
   buildFeatures **{  
**     viewBinding = true  
   }  
}

Now, just use binding instance and you will get the view references in camel casing just the way a coding standard says. :-)

**c) DataBinding:** With ViewBinding, you can’t use it to bind layouts with data in XML (No binding expressions, no BindingAdapters nor two-way binding with ViewBinding). For that we use DataBinding. Just the same way, add this your app  **build.gradle:**

android {  
    ...  
  buildFeatures **{** dataBinding = true  
  }  
}

Wrap your layouts with <**layout**> tag in XML and get the view references like ViewBinding.

From the above, We can deduce that:

-   **ViewBinding**  = Only binding views to code
-   **DataBinding**  = Binding data (from code) to views + ViewBinding (Binding views to code)

> There are many other differences as well. Check  [here](https://medium.com/@hardianbobby/databinding-vs-viewbinding-simple-comparison-792fa8d72e8)  and choose wisely. :-)

I am using both in my project according to the requirements.

# ViewBinding

**Under the hood:**

If you look at the source code,  _View Binding_  is very easy to understand.  _It_ will generate one  **binding object**  for every XML layout in your module.

> Inflate method inside the binding class contains a  **bind**  method which eventually calls  **findViewById** for each view to bind. With providing specific id to each view, it ensures the type-safety and annotations like  `@Nullable`  or  `@NonNull`  helps in providing null-safe types.

A few days back, I looked at the source code for  **How DataBinding works under the hood** and thought of sharing my learnings and understanding through this article.



# Android-Interview-Questions

[![interview](https://github.com/niharika2810/android-interview-questions/raw/master/images/interview.png)](https://github.com/niharika2810/android-interview-questions/blob/master/images/interview.png)

A repository containing interview questions on DS, Java & Android based on my experiences.

**Note**: I am not writing up the answers here for now as If I provide you the answers, what will you do? :-)

Also, you will restrict yourself to those answers only. Try and learn more deeply abut the concepts. For any particular questions, You can ask anytime.

## DS

1.  [](https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/)[https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/](https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/)
    
2.  [](https://www.geeksforgeeks.org/largest-subarray-with-equal-number-of-0s-and-1s/)[https://www.geeksforgeeks.org/largest-subarray-with-equal-number-of-0s-and-1s//](https://www.geeksforgeeks.org/largest-subarray-with-equal-number-of-0s-and-1s//)
    
3.  [](https://www.geeksforgeeks.org/find-triplets-array-whose-sum-equal-zero/)[https://www.geeksforgeeks.org/find-triplets-array-whose-sum-equal-zero/](https://www.geeksforgeeks.org/find-triplets-array-whose-sum-equal-zero/)
    
4.  [](https://www.geeksforgeeks.org/level-order-traversal-in-spiral-form/)[https://www.geeksforgeeks.org/level-order-traversal-in-spiral-form/](https://www.geeksforgeeks.org/level-order-traversal-in-spiral-form/)
    
5.  [](https://www.geeksforgeeks.org/construct-tree-from-given-inorder-and-preorder-traversal/)[https://www.geeksforgeeks.org/construct-tree-from-given-inorder-and-preorder-traversal/](https://www.geeksforgeeks.org/construct-tree-from-given-inorder-and-preorder-traversal/)
    
6.  [](https://www.geeksforgeeks.org/write-an-efficient-c-function-to-convert-a-tree-into-its-mirror-tree/)[https://www.geeksforgeeks.org/write-an-efficient-c-function-to-convert-a-tree-into-its-mirror-tree/](https://www.geeksforgeeks.org/write-an-efficient-c-function-to-convert-a-tree-into-its-mirror-tree/)
    
7.  [](https://www.geeksforgeeks.org/maximum-sum-such-that-no-two-elements-are-adjacent/)[https://www.geeksforgeeks.org/maximum-sum-such-that-no-two-elements-are-adjacent/](https://www.geeksforgeeks.org/maximum-sum-such-that-no-two-elements-are-adjacent/)
    
8.  [https://www.geeksforgeeks.org/longest-common-subarray-in-the-given-two-arrays/?ref=leftbar-rightbar](https://www.geeksforgeeks.org/longest-common-subarray-in-the-given-two-arrays/?ref=leftbar-rightbar)
    
9.  [https://www.geeksforgeeks.org/maximum-sum-path-across-two-arrays/](https://www.geeksforgeeks.org/maximum-sum-path-across-two-arrays/)
    
10.  [https://www.geeksforgeeks.org/merge-two-sorted-arrays-o1-extra-space/](https://www.geeksforgeeks.org/merge-two-sorted-arrays-o1-extra-space/)
    
11.  [https://www.geeksforgeeks.org/segregate-0s-and-1s-in-an-array-by-traversing-array-once/](https://www.geeksforgeeks.org/segregate-0s-and-1s-in-an-array-by-traversing-array-once/)
    
12.  [https://www.geeksforgeeks.org/construction-of-longest-monotonically-increasing-subsequence-n-log-n/](https://www.geeksforgeeks.org/construction-of-longest-monotonically-increasing-subsequence-n-log-n/)
    
13.  [https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/)
    
14.  [https://www.geeksforgeeks.org/find-zeroes-to-be-flipped-so-that-number-of-consecutive-1s-is-maximized/](https://www.geeksforgeeks.org/find-zeroes-to-be-flipped-so-that-number-of-consecutive-1s-is-maximized/)
    
15.  [https://www.geeksforgeeks.org/reverse-a-doubly-linked-list/](https://www.geeksforgeeks.org/reverse-a-doubly-linked-list/)
    
16.  [https://www.geeksforgeeks.org/longest-common-increasing-subsequence-lcs-lis/](https://www.geeksforgeeks.org/longest-common-increasing-subsequence-lcs-lis/)
    
17.  [https://www.geeksforgeeks.org/fix-two-swapped-nodes-of-bst/](https://www.geeksforgeeks.org/fix-two-swapped-nodes-of-bst/)
    
18.  [https://www.geeksforgeeks.org/backttracking-set-2-rat-in-a-maze/](https://www.geeksforgeeks.org/backttracking-set-2-rat-in-a-maze/)
    
19.  [https://www.geeksforgeeks.org/detect-and-remove-loop-in-a-linked-list/](https://www.geeksforgeeks.org/detect-and-remove-loop-in-a-linked-list/)
    
20.  [https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/](https://www.geeksforgeeks.org/find-minimum-element-in-a-sorted-and-rotated-array/)
    
21.  [https://www.geeksforgeeks.org/maximum-product-subarray/](https://www.geeksforgeeks.org/maximum-product-subarray/)
    
22.  [https://www.geeksforgeeks.org/merge-k-sorted-arrays/](https://www.geeksforgeeks.org/merge-k-sorted-arrays/)
    
23.  [https://www.geeksforgeeks.org/sort-an-array-of-0s-1s-and-2s/](https://www.geeksforgeeks.org/sort-an-array-of-0s-1s-and-2s/)
    
24.  [https://www.geeksforgeeks.org/a-product-array-puzzle/](https://www.geeksforgeeks.org/a-product-array-puzzle/)
    
25.  [https://www.geeksforgeeks.org/count-frequencies-elements-array-o1-extra-space-time/](https://www.geeksforgeeks.org/count-frequencies-elements-array-o1-extra-space-time/)
    
26.  [https://www.geeksforgeeks.org/median-of-two-sorted-arrays-of-different-sizes/](https://www.geeksforgeeks.org/median-of-two-sorted-arrays-of-different-sizes/)
    
27.  [https://www.geeksforgeeks.org/find-maximum-path-sum-in-a-binary-tree/](https://www.geeksforgeeks.org/find-maximum-path-sum-in-a-binary-tree/)
    
28.  [https://www.geeksforgeeks.org/lowest-common-ancestor-in-a-binary-search-tree/](https://www.geeksforgeeks.org/lowest-common-ancestor-in-a-binary-search-tree/)
    
29.  [https://www.geeksforgeeks.org/lowest-common-ancestor-binary-tree-set-1](https://www.geeksforgeeks.org/lowest-common-ancestor-binary-tree-set-1)
    
30.  [https://www.geeksforgeeks.org/sorted-linked-list-to-balanced-bst/](https://www.geeksforgeeks.org/sorted-linked-list-to-balanced-bst/)
    
31.  [https://www.geeksforgeeks.org/given-an-array-a-and-a-number-x-check-for-pair-in-a-with-sum-as-x/](https://www.geeksforgeeks.org/given-an-array-a-and-a-number-x-check-for-pair-in-a-with-sum-as-x/)
    
32.  [https://www.educative.io/edpresso/how-to-find-the-most-frequent-word-in-an-array-of-strings](https://www.educative.io/edpresso/how-to-find-the-most-frequent-word-in-an-array-of-strings)
    
33.  Continuation Question to  `32`, What is the time complexity of your solution and how can you improve it?
    
34.  What is the time complexity of mergeSort, quickSort and Binary search?
    
35.  What is the time complexity for removing an element from LinkedList?
    

## [](https://github.com/niharika2810/android-interview-questions#java)Java

-   Interfaces vs Abstract Classes.Why Interfaces if abstract classes are already there.
    
-   JAVA 8 new features
    
-   Hashmap implementation,ArrayList implementation
    
-   How to make a class immutable.
    
-   String pools,intern keyword,new() keyword
    
-   Linked list vs Array,Array vs ArrayList,Set vs List vs Map,Iterator vs Enumeration.
    
-   How to compare two objects in Set?(hint : equals() and hashcode() overriding)
    
-   How ClickListener implemented in Java?
    
-   Programs based on Inheritance and multithreading. (Most Important)
    
-   1.) Add(String string) 2.) Add (Object object), you called Add(null) what will be the output here?
    
-   Why main Static?
    
-   Static vs Singleton? Which to prefer when?
    
-   Can a static method be overloaded/overridden?Can a constructor be made static? Can we override/overload constructor? Which type of error you get-Compile/Runtime.
    
-   Can we have final/static/default methods in interface/abstract classes?
    
-   Check for every keyword? Which type of error-Compile time or runtime(I remember it after I tried a sample in Android studio).
    
-   Stack vs Heap allocation in Java.
    
-   Why is String immutable?
    
-   Why reflection in Java?Introspection vs Reflection?
    
-   Anonymous vs Inner Classes.Static Inner Classes. Can we have Static inner classes on Top?
    
-   What is the naming structure for an inner class in java?
    
-   Default value for Access specifiers,primitie types.
    
-   Protected vs Default. Do static retain value if an application is removed from recent.
    
-   How is the Exception class implemented in java?Try/Catch with finally block.
    
-   Any situation when finally won’t get executed?
    
-   Is Java pass by value/reference?
    
-   What will be the result if StringBuffer value compared with String — strbuf.equals(str) /str.equals(strbuf)
    
-   Synchronization in Java.
    
-   Can one interface extend/implement other/Can one abstract class extend other? Can interface extend abstract classes?
    
-   Runtime vs compile time polymorphism with real-world scenarios. How do you define encapsulation?
    
-   this vs super() keyword
    
-   Implement own Garbage collection in java.
    
-   Why runnable interface if we have already had Thread class in Java?
    
-   Why thread pool in Java?
    
-   Order of call of constructors during Inheritance.
    

## [](https://github.com/niharika2810/android-interview-questions#kotlin)Kotlin

-   Why should we use Kotlin for Android Development?
    
-   Kotlin operators - apply, run, let, ?
    
-   How Kotlin is null-safe?
    
-   What is the difference between var and val?
    
-   How const is different from val?
    
-   What is the difference between ?. and !!
    
-   How to define static functions in Kotlin?
    
-   What are Data Classes in Kotlin?
    
-   What do you mean by Sealed classes in Kotlin?
    
-   Collections in Kotlin.
    
-   Difference between lateinit and lazy.
    
-   What is Kotlin synthetic binding?
    
-   Difference between Kotlin Synthetics, View binding and Butterknife?
    
-   What are extension functions in Kotlin?
    
-   What are lambda expressions?
    
-   What are Higher-Order functions in Kotlin?
    
-   Difference between let, run, with, also, apply in Kotlin.
    
-   What is the best way for performing tasks on a background thread in kotlin?
    
-   What are Coroutines in Kotlin?
    
-   What are the different Coroutines Scope?
    
-   Why use suspend function in Kotlin Coroutines?
    
-   Explain Coroutine LifecycleScope.
    
-   What is Kotlin Flow?
    
-   Kotlin Flow vs RxJava.
    
-   Room vs Realm? Why not Realm?

# Android and Kotlin conference videos

This repo contains collection of talks (video playlists) from various Android and Kotlin related conferences.

It will be handy in case you missed any of the sessions or just want to go re-watch the recordings. Enjoy!

## [](https://github.com/niharika2810/android-kotlin-conference-videos#2020)2020

### [](https://github.com/niharika2810/android-kotlin-conference-videos#android)Android

-   [Droidcon APAC (Dec 2020)](https://www.droidcon.com/videos?path=%20droidcon%20APAC)
-   [MobileOptimized (Nov 2020)](https://www.youtube.com/playlist?list=PLpVeA1tdgfCAEG_WDyLKoHDxmsGocQaX6)
-   [Droidcon Americas (Nov 2020)](https://www.droidcon.com/videos?path=droidcon%20Americas)
-   [Android Summit (Oct 2020)](https://www.youtube.com/playlist?list=PLzJZrgVJE8BYZvsHFe2M3FjjTmjbcT6hH)
-   [Droidcon EMEA (Oct 2020)](https://www.droidcon.com/videos?path=droidcon%20EMEA)
-   [DevFest UK & Ireland (Oct 2020)](https://www.youtube.com/playlist?list=PLGCUisAoTVvFAZPVqSx54snMBTXw798Jr)
-   [Android Security Symposium (Jul 2020)](https://www.youtube.com/playlist?list=PL61IkVbNYniUTmprGxMnlUFxmFj79Wmpw)
-   [360|AnDev (Jul 2020)](https://www.youtube.com/playlist?list=PLnD_TKDSaFyXWrnnEhfxeKABuq49Is-8o)
-   [Android Makers (Apr 2020)](https://www.youtube.com/playlist?list=PLn7H9CUCuXAsILGb3mNo654e2G-d9K_I1)
-   [Droidcon Online (Apr 2020)](https://www.droidcon.com/videos?path=droidcon%20Online)

### [](https://github.com/niharika2810/android-kotlin-conference-videos#kotlin)Kotlin

-   [Kotlin 1.4 Online Event (Oct 2020)](https://www.youtube.com/playlist?list=PLQ176FUIyIUankIQrXKNfXaOxOPx04D8V)
-   [47 Degrees Academy (Jun 2020)](https://www.youtube.com/playlist?list=PLTx-VKTe8yLyr2ExNXf6O81C07GJ6WgV1)
-   [Kotliners (Jun 2020)](https://www.youtube.com/watch?v=5qcpq6jnrXI&list=PLnYRVL0Cw1FQRDYpKQ8kbcg2-K8I9k1RH)

# Android Learning Resources

## [](https://github.com/niharika2810/android-learning-resources#pick-up-the-best-and-save-it-for-future-reference)Pick up the best and save it for future reference.

[![learning](https://github.com/niharika2810/android-learning-resources/raw/master/images/learning.jpg)](https://github.com/niharika2810/android-learning-resources/blob/master/images/learning.jpg)

We struggle a lot learning small, basic and specific concepts. Here is my list of resources which may help you understand  **Android**  in a better way. A repository containing link/resources to small, basic as well as specific Android concepts.

## [](https://github.com/niharika2810/android-learning-resources#java)Java

-   Is Java pass by reference or value?
    
    [Link 1](https://www.infoworld.com/article/3512039/does-java-pass-by-reference-or-pass-by-value.html#:~:text=Java%20always%20passes%20parameter%20variables,is%20passed%20to%20a%20method.&text=%E2%80%9CPassing%20by%20reference%E2%80%9D%20refers%20to,of%20the%20variable%20in%20memory.)
    
    [Link 2](https://www.journaldev.com/3884/java-is-pass-by-value-and-not-pass-by-reference)
    
-   [Desugaring in Java](https://blog.mindorks.com/desugaring-in-android)
    
-   [Understanding Overloading and Overriding in Java](https://java2blog.com/method-overloading-and-overriding-interview-questions-in-java/)
    
-   [Practive Overloading and Overriding Java](https://javaconceptoftheday.com/java-practice-questions-on-method-overloading-and-overriding/)
    
-   When to use Static class and when Singleton?
    
    [Link 1](https://javarevisited.blogspot.com/2013/03/difference-between-singleton-pattern-vs-static-class-java.html)
    
    [Link 2](http://www.crazyforcode.com/cant-static-class-singleton/)
    
-   OnClickListener Internals
    
    [Link 1](https://stackoverflow.com/questions/32884665/how-does-androids-setonclicklistener-works)
    
    [Link 2](https://stackoverflow.com/questions/11815101/view-onclicklistener-a-function-or-interface)
    
-   [Hashmap Internals](https://www.javatpoint.com/working-of-hashmap-in-java)
    
-   [ArrayList Internals](https://www.netjstech.com/2015/08/how-arraylist-works-internally-in-java.html)
    
-   [LinkedList Internals](https://www.netjstech.com/2015/08/how-linked-list-class-works-internally-java.html)
    
    [Link 2](https://dzone.com/articles/linked-list-journey-continues)
    
-   Reflection in Java
    
    [Link 1](http://tutorials.jenkov.com/java-reflection/index.html)
    
    [Link 2](https://www.edureka.co/blog/java-reflection-api/)
    
-   [Garbage Collection](https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/)
    
-   Why Java is platform independent?
    
    [Link 1](https://stackoverflow.com/questions/541487/implements-runnable-vs-extends-thread-in-java#:~:text=When%20you%20extends%20Thread%20class,same%20object%20to%20multiple%20threads.)
    
    [Link 2](https://www.geeksforgeeks.org/java-platform-independent/)
    
-   Implements Runnable vs extends Thread
    
    [Link 1](https://medium.com/@neil.wilston123/why-java-is-platform-independent-1d82c2249a69#:~:text=Irrespective%20of%20the%20platform%20the,is%20called%20platform%20independent%20language.&text=oriented%20programming%20language.-,It%20is%20a%20platform%20independent%20language%20i.e.%20the%20compiled%20code,on%20any%20java%20supporting%20platform.)
    
    [Link 2](https://www.geeksforgeeks.org/implement-runnable-vs-extend-thread-in-java/)
    
-   [Sparse Array vs Hashmap](https://stackoverflow.com/questions/25560629/sparsearray-vs-hashmap#:~:text=A%20sparse%20array%20in%20Java,is%20a%20key%2Cvalue%20pair)
    
-   [Static vs Singleton](https://stackoverflow.com/questions/3532161/what-is-the-difference-between-a-singleton-pattern-and-a-static-class-in-java)
    
-   [Lambda vs Anonymous Classes](https://stackoverflow.com/questions/22637900/java8-lambdas-vs-anonymous-classes)
    
-   [Stack vs Heap Memory](https://www.journaldev.com/4098/java-heap-space-vs-stack-memory#:~:text=Difference%20between%20Java%20Heap%20Space%20and%20Stack%20Memory,-Based%20on%20the&text=Heap%20memory%20is%20used%20by,contains%20the%20reference%20to%20it)

