---
date: "2020-02-12"
title: "Helm Charts - My first clumsy attempt"
slug: "helm-charts-attempt"
tags: []
categories: [tech, kubernetes, helm]
comment: true
---

![Helm](/images/helm.png)

When I started my journey into Kubernetes back in 2018, I remember people talking about Helm as the easiest way to deploy applications into Kubernetes. I remember the tool was a favourite for developers and I got curious about it and started to look more into it.

It was with a heavy heart that I decided to drop Helm altogether at the time. 

The main concerns I had were about:
- Security around the Tiller component
- A non-purist approach towards Kubernetes declarative manifests

The first reason is pretty self-explanatory since Tiller was also described as one giant sudo server. The second one was around the argument that "cloud-native" application should be able to gracefully handle dependencies that takes more time to be deployed either by implementing a retry logic or other means. 

What changed my mind now? Well, the advent of helm 3

## Helm 3 - A Tillerless world

To my surprise, Helm 3 was released without the need of having Tiller deployed in Kubernetes. 

You can read more about all the changes [here](https://helm.sh/docs/faq/) and in case you didn't know, you can also read more in detail about the security concerns around Tiller.

This was incredibly liberating since it was my biggest concern with the tool so far and combined with the fact that I was getting a little bit tired of having to write Kubernetes manifests with very repeatable fields, it made adopting helm all the more compelling.

Helm templating was anyway a great feature you could use if you didn't want tiller in your cluster and many people were using it that way, but because my work on Kubernetes was more project focused I never had a big need for it until quite recently.

## The bad things

Finally getting started with Helm proved to be more difficult than I hoped.
Although plenty of charts were out there ready to be consumed, developing one was not as easy as I originally believed.

### The problem with documentation

I found it quite hard to navigate the documentation. Although very complete and descriptive about the features available to Helm, I found it hard to read it through since sometimes it feels more like a collection of blog posts.

The way it is also formatted makes it hard to find the information you're seeking and the side menu that keeps on closing every time you click an element is a little irritating. 

A great deal of improvement could be made by just adjusting the website font size and menu, which hopefully is something other people can agree with.

### The problem with secrets

Another big issue I found with Helm is the lack of management of the secrets or more specifically generated secrets. There is a whole thread opened in [github](https://github.com/helm/charts/issues/5167) if you want to read about it, but in essence, at the moment of my writing, you shouldn't use Helm to define your secrets.

I stumbled upon a nice way of generating random password in a helm way, but I soon found out that successive releases were breaking because a new randomly generated password would be pushed into my Kubernetes secrets and would break functionality.

This is because Helm doesn't handle secrets. It was very confusing for me since many charts had randomly generated password for defaults and I didn't notice until I started to play around with it. 

For my charts, secrets are at the moment statically defined by the values yaml file. I believe it's the best way to have a chart user being more aware of what's going on. Because Helm doesn't do secret management, the best way I believe to move forward is to define secrets externally and load them at runtime during deployment in a pipeline.

### The problem with ready to use charts

The charts that can be found under the stable repository of Helm are very nice, definitely really good to get started quickly, but a bad idea to use "AS IS" for production.

The biggest problem I have with them is the over-generalization, which is a must if you want to share your work with other people since what works for you may not work with everyone else depending on different configurations and other factors, but it's daunting when you want to adopt it in your company.

It becomes a big vetting process, where you try to understand what the chart is doing and why is it doing it in that way. This becomes even more difficult given the fact that Charts are incredibly hard to read especially with if/else logic and obscure template function you may not know about.

### The lack of an out of the box diff feature

This is not really on Helm, as I believe this is not available out of the box in Kubernetes as well, but I believe it could be a cool feature to implement out of the box in the tool. I know there is a plugin that does this and I haven't tested it yet, but since it is possible I believe it would be nice to have it in Helm.

This opinion comes from a heavy Terraform user, which is an incredible tool that gives you the ability to see what is going to happen before you push your infrastructure as code into production (or any other environment). It let me know what is going to change, which is a great way to double-check if I'm doing something wrong. 

If Helm had a feature like that out of the box, I believe it could be the killer feature that would push the whole community to adopt helm as a standard for deployment in Kubernetes.

## The good things 

Helm is an incredible tool that in my opinion fits quite well with the Kubernetes world.

It enables you to create reusable deployments, package it and share it within the company or the community. I also believe it is the only tool so far that makes this possible without having to write additional scripts.

I previously mentioned how Helm approach was "non-purist", but I learned over time that in real-world scenario it can be quite difficult to have the time to implement the necessary logic to make the application smarter. Sometimes it's just difficult to convince management that it is worth the time, and sometimes it's just not that trivial. That's why the dependency management of Helm comes out as a winner since the outcome is faster implementation and delivery to production.


## Conclusion

Although I wrote more about the bad instead of the good, Helm still comes out on top as the tool to use for deployment in Kubernetes. There is no valid alternative at the moment and so far it's the best way to properly package and standardise your deployment.

It could be further polished, which is what all of the "bad things" I described are all about, but overall it's pretty good.
This is, of course, coming out of a 2-week experience with Helm so I could change my mind in the future (which I will, of course, write about in this blog) but for what I see, the tool can only improvement so I don't think I will.