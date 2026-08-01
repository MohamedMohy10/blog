---
layout: post
title: "How to Get Better Results from LLMs: 3 Practical Tips"
date: 2026-07-30
description: "3 practical tips to get more consistent, higher-quality results from LLMs"
---

If you are still struggling in getting satisfying results from LLMs for your work .. things are not done perfectly or not exactly the way you wanted .. you should know that one of the most important skills for getting better results from these models is prompt engineering (or, more broadly, learning how to provide the right context and instructions). 
You must learn this skill, and in some cases it might not be as easy as it seems. 

There are many discussions today about whether prompt engineering is becoming a new form of programming. Some people even argue that it is becoming one of the most important skills to have in the AI era, while others disagree with comparing it to actual programming languages like C++ or Python. Personally, I don't think the comparison itself matters much .. what matters is that knowing how to communicate effectively with LLMs is becoming more and more useful, whatever you want to call it. 

So whether you are still a beginner developer in your prompt engineering journey, a vibe coder, or even someone outside the tech field who uses LLMs for work .. here are 3 tips that may help you improve your results.

## 1. Give it some time

Yes, you have to spend time actually using the model and watching how it behaves. The more you use a certain model, the more you start understanding what kind of instructions it responds well to, how much context it needs, and how small changes in wording can shift the output .. that's why you'll find people getting very good results from a model they use often, while getting worse results the first time they try a different one .. it's not that one model is smarter, it's that they've built better intuition for communicating with that specific one. 
The good news is a lot of this knowledge is transferable .. models aren't identical but they do share a lot in common, So as you gain experience with one model, some of what you learn will help you with others to a good degree.

So do not rush. Do not expect yourself to immediately know the perfect way to prompt a model. Use it, experiment with it, try explaining the same thing in different ways, see what actually changes the output and what doesn't.

And one important point here: do not judge a prompt based on one successful answer. Sometimes a prompt works well simply by chance. Try it with different examples and see whether you can get consistently good results.

Over time, you build your own intuition, and that's the real goal here.

## 2. Get help from LLMs while improving your prompting skills

This is something I personally find very useful.

Let's say you are going to use Claude for a project or a task. Go to another model first (ex. ChatGPT), and tell it your whole story and what you want, explain the entire problem and what you want to achieve .. and please be specific and precise. Do not ever pretend that the LLM will understand what's in your mind without you needing to write it. Tell it what you're trying to do, what you already have, what the expected result is, what constraints you're working with, and anything else relevant.

Then, instead of just asking "do you understand?" .. ask it something like: "restate the task back to me in your own words, list out the requirements and assumptions you understood, point out anything missing or unclear, and ask me any questions you need before doing anything else." An LLM can easily tell you it understands while still missing something important .. this forces it to actually show you.

Once everything is clear, ask it for a full plan for the task .. and please review the plan critically (sorry for vibe coders but you will have to do some work here and have some understanding of what you're actually doing). Look for anything missing, anything that needs modification, steps that aren't necessary or may add unwanted complexity (and this happens more than you'd think). Otherwise you might end up approving a plan that looks impressive but doesn't actually solve your problem.

After you resolve all the issues with ChatGPT and agree on a final plan .. ask it to make a prompt for Claude to implement it. And again, do not follow blindly. Review the prompt, read it, try to understand the patterns and how the words were chosen, compare it with what you originally wrote describing your problem. Look if there is something missing, something needs to be rephrased, whether there is unnecessary information or details or something that should be removed, a requirement that is not mentioned

This is how you learn .. you are not just getting a better prompt for your current task, you are also training your brain to write better prompts on your own. 
And do not worry, I'm not expecting you to catch everything at this early stage .. but after doing this repeatedly, you will probably find yourself reading the LLM-generated prompt and immediately thinking: "This whole paragraph is unnecessary" or "This sentence is ambiguous" or "That should have been mentioned earlier." or "This part is missing completely." That's the point .. eventually you'll need the LLM less and less for this part. 

Once you're satisfied with the prompt, give it to Claude.

**Optional step: use another model as a reviewer**
If the task is very important or you just want more precision, you can add another review layer .. take the prompt you created with ChatGPT and give it to another model (ex. DeepSeek, Gemini, etc.), give it short context on the task if needed, then ask it to review the prompt for missing requirements, ambiguity, unnecessary complexity, or anything that might lead to a bad implementation. This can sometimes be useful as different models sometimes catch different things. 
But i wouldn't turn this into a chain of 5 models reviewing each other for every small task .. at some point you're just adding complexity for no real gain. 
If this process starts making you lose focus or feel overwhelmed, stick with one model .. you're already in a good position doing that.

## 3. Use one LLM to review the work of another

After Claude implements the prompt .. if you know how to evaluate the result yourself, do that. But if you're not sure, again, you can use another LLM. 
Take what you got from Claude and go back to ChatGPT and ask it to review the results, identify problems, missing requirements, bugs, inconsistencies, or anything that does not match the original goal .. or in some cases, ask it to tell you *how* to evaluate it. This is a well-known technique called "LLM as a judge", where one LLM is used to evaluate the output of another LLM.

You can also -same as the optional step in tip 2- use multiple models as "judges" if the task is important or you want to get different perspectives or insights. But here's an important point .. LLMs can share the same blind spots, and one model can also convince another model that a wrong criticism is correct. So treat their feedback as another source of evidence, not as absolute truth.
If several models point out the same issue, that may be a good signal that you should look into it .. but the real priority should depend on the actual requirements of your task and, whenever possible, objective tests or verification. 

For example, if you are writing code, running tests is more valuable than asking 5 LLMs whether the code looks correct.. If you are writing a report, checking the sources is more valuable than simply asking another LLM whether the report is accurate. 
The LLM can point you toward what to investigate .. but you still have to verify it yourself.

Once you have identified the issues, go back to your planning model (ChatGPT here) and ask it to create a new prompt that addresses them, send this prompt to your implementation model again (Claude), then evaluate the result again .. and the iteration continues until you reach something that actually satisfies your requirements.

## One more thing worth mentioning: don't blindly send everything to every model

Since this whole workflow means moving information between multiple LLMs, you should also think about privacy as many people overlooks this while it is quite important. 
Don't paste your information, confidential company info, private customer data, passwords, API keys, sensitive source code, personal records, or anything you're not supposed to share with an external service. Using multiple models can genuinely improve your workflow, but you still need to know where your data is going and what your organization actually allows.
So please care about yours and others privacy.

## Disclaimer

1. The tips described above are not meant to replace prompt engineering an do not change the fact that you should learn prompt engineering yourself .. these are just extra tools to use and also sharpen your prompting.

At the beginning, you may need another LLM to help you formulate your thoughts .. organize your requirements .. and create a good prompt.
But after doing this many times, you will start noticing patterns .. you will begin to understand what information models need .. you will become better at explaining your requirements .. and you may even recognize ambiguity before the model points it out. 
By experience, you'll realize that sometimes two solid, well-structured lines written by you get far better results than a whole paragraph generated by an LLM.
You will discover that more words do not automatically mean a better prompt, and more context does not automatically mean better results either. The main idea is to give the model the right information, in the right structure, with the right level of detail.

2. The models mentioned here are not a requirment .. I have mentioned Claude and ChatGPT just for demonstration, but you can — and actually are encouraged to — use whichever models you're comfortable with and whichever ones you find suitable for your task.
