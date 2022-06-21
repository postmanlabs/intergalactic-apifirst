# Foundations: What is API-First

The purpose of this session is to level-set all attendees with the terminology and phrases that you'll hear throughout the day. We're going to start with what API-First is all about, and follow up with some background on OpenAPI and how it helps with API-First.

You may be familiar with some of this content already, and that's great!

## API-First is a Important Direction

APIs are an important part of the industry, connecting businesses to one another, and connecting businesses to users. We need to think about what that experience is going to be like for the users and developers utilizing these APIs.

"Mobile-First" saw a lot of movement in the tech industry a few years back, where companies would plan ahead to make mobile device experiences as great as possible.

Likewise, the concept of "API First" is becoming more popular. The idea here is to think ahead, plan ahead, and design what your user experience will be before you implement anything about your API.

## The API Lifecycle

At Postman, we think the API Lifecycle is very integral to building a great API. Just like typical Agile Methodology principles of planning, developing, and feedback, all running in a cycle, the idea of API-First will work in a similar fashion.

The difference we're discussing today is that the feedback and changes you implement will be only around the definition and design of the API before you ever build the API itself.

### The Mindset

Focus on your business needs of the API for a moment. What is the end goal? What is this API to be used for? Who is the end user, and what should they expect as they use this API? This is more important than the technologies you use to build your API.

### Plan the API and iterate on the Design

As you plan the API, think about how you will build common responses, error message structure, status codes, and more. Does that fit the business goal?

Use this feedback to refine your plan. Then think of ways you can re-use designed elements like the structure of the response message, or the structure of the error message, so you can avoid duplicating that effort.

### "Free Your Mind", aka Decoupling Legacy Constraints

Try to avoid building your next API the "same way we've always done it." Think about this next API in a different way that focuses on the business needs, the user, the interfaces, before worrying about legacy systems and how they need to communicate.

This is easier said than done for some organizations, but the idea of API-First is that you're planning for the user experience and the business needs ahead of the implementation.


## Commonality to "-Drive Development" systems

API-First will, at a glance, appear to be very similar to Document-Driven or Design-Driven Development. You would be correct in this thinking.

Shifting to an API-First Mindset, when you already have systems in place, is harder, but organizations like [Etsy](https://www.infoq.com/news/2016/08/etsy-api-first-architecture/) and [Netflix](https://netflixtechblog.com/optimizing-the-netflix-api-5c9ac715cf19) have found ways to accomplish this by building APIs with feature parity working in parallel with existing systems.

## API-First in Practice

The API-First approach will test, in no small part, the effectiveness of communication within your organization.

### Governance

Building a "governance" team to organize the rules and conventions of your APIs going forward will be an important step. This group will plan the consistency of the APIs for everything from design to security to the responses, and more.

### Contracts, but with less Legal Department

Once you plan everything, your team can define the "contract" of the API -- what the endpoints will do, and what the responses should be. Later, your team can use these to build robust tests to ensure everything follows this "contract".

### Source of Truth

Your organization needs one place to store and maintain the design and rules, and everyone copies from that source. This must be easy to find within your organization, it must be well documented, and ideally, searchable for content.

---

## OpenAPI as a Planning Tool

Originally known as the Swagger Specification, the [OpenAPI Specification (OAS)](https://www.openapis.org/) is a format that can be used to describe, produce, consume, and visualize RESTful web services. It is a specification standard for REST APIs that defines the structure and syntax.

Although the OpenAPI Specification is currently at v3.1, we will be focusing on v3.0 today. This portion of the session will be giving an introduction to OpenAPI, and our next session will start to build a new API together using OpenAPI.

### The Specification

The OpenAPI Specification is a machine-readable and technology-agnostic standard which is also vendor-nerutral and human-friendly. It's written in JSON or YAML format and generally encourages the use of JSON Schema to define data.

## The Difference between "OpenAPI" and making an "open API"

Using the OpenAPI Specification does not imply that you have to build an "open" API, accessible to everyone in public. The word "Open" in the OpenAPI name means that the specification itself is [open for discussion and contribution](https://github.com/OAI/OpenAPI-Specification/blob/main/GOVERNANCE.md).

## What can you do with OpenAPI

Once you have a specification built, tools exist to generate a large number of things:

- documentation
- design
- mock servers
- code generation (server-side and client-side SDKs)
- testing and contract validation
- ... more!

## How does OpenAPI help drive API-First

OpenAPI is useful along the whole API Lifecycle

- Design everything before any software implementation happens
- Governance, planning your standardization princples to execute the development
- Feedback for the definitation, design, documentation, and influence other areas of the API Design Lifecycle
- Acts as your single "source of truth"
- Contracta and Validators

OpenAPI is a "bridge" between people, and the API tools. It helps to give an understanding about the behvaior of an API, whether or not it's even built yet.

