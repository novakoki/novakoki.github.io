---
title: Memory Leak
date: 2015-02-04
locale: en_US
---

> Those who cannot remember the past are condemned to repeat it.

The title seems to suggest I was about to write a technical article, but given my limited knowledge (I haven't even studied operating systems, yet here I am rambling!), this is just an ordinary essay playing with concepts. Why "Memory Leak"? Because I'm genuinely in a state of memory leakage.

Since the summer after my freshman year, the following code describes my actual state:

```cpp
typedef learning_area T;
while (found_new_interest){
    T* p = new learning_area(); // Hey, your pointer is gone after one use!
    p->learn_three_days();
    p->rest_two_days();    // Hey, where's your delete?
} // Hey, wake up, wake up!
```

And then I could only restart by sleeping.

Let me count how many wild pointers I have:

* Web Development (including HTML, CSS, JavaScript, PHP, Bootstrap... blah blah)
* Python (now also involved with Web)
* Git (version control, haven't used any fancy features, purely as a code repository)
* TeX (will I use it for writing papers in the future?)
* Network Security (XSS, SQL injection, nmap, burp, password dictionary brute force - seems like I've never succeeded)
* Data Structures and Algorithm Analysis (whatever happened to seriously studying algorithms in the second semester of sophomore year?)
* Robotics
* Graphics and Images (OpenCV?)
* Machine Learning (looked at Andrew Ng's course during winter break)

Add Android from the second semester of freshman year, and well, memory has overflowed.

Okay, the first phase of memory cleanup is finally complete.

Let me summarize the issues:

* No output, and with memory not being enough, there's always hard disk
* Always fishing for three days and sun-drying for two, nothing substantial, falling apart when someone asks a question
* Input range too wide, resource-intensive syndrome. This truly is the best era, with MOOCs, documentation, blogs in abundance, but also extremely difficult when it comes to choosing

Alright, wild pointers are flying again. Time to restart, continue memory cleanup tomorrow.
