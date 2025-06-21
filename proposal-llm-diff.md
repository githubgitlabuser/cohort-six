# Differential LLMing

A tool using LLMs to analyze implementations of EIPs and consensus-specs between the different clients to find differences in implementation.

## Motivation

`What problem is your project is solving? Why is it important and what area of the protocol will be affected?`

During implementation of an EIP, various client teams do their implementation at their own pace and making their own design decisions. This can potentially lead to the following at times:

- Potential attack vector in case of a security vulnerability 
- Edge cases which might lead to bugs
- Accidental deviation from the original EIP specs that might be hard to miss for humans
- A sub-optimal design decision that might affect performance

Currently the core devs collaborate among each other once every two weeks to discuss ideas and potentially learn from one another. This helps in mitigating the above issues. But there might still be issues that are probably only found out much later (e.g. duing an audit) 

## Project description

`What is your proposed solution?`

The proposal is the create an LLM based program that augments this whole process that automates at least some parts of the above. Once you specify the EIP and clients that you are interested in, the program will create a useful and detailed report on how the implementations differ and especially if there is any cause of concern in any of the implementation. 

## Specification

`How will you implement your solutions? Give details and more technical information on the project.`

Overall it will have a frontend and a backend. 

- A simple frontend that will take the following:
    - EIP number
    - A selection of clients
- A backend that does the following:
    - Takes in the input from frontend
    - Fetches the EIP specs and creates a summary
    - Analyzes each client's implementation of the EIP by checking the PR diff 
    - Creates a final report to showcase the similarities, differences of implementation, potential edge cases and cause of concerns 

## Roadmap

`What is your proposed timeline? Outline parts of the project and insight on how much time it will take to execute them.`

First stage: Simplest form that works (1-2 weeks)
Create a first PoC that has the following:
- Basic frontend
- Backend with support for single PRs per client for simplicity 
- Extraction of core EIP specifications and creation of summary along with manual checks if it works fine
- Extraction of PR diffs and creating a meaningful summary of it including relevant important codebase 
- Using the summaries generate a comparison report in markdown about the EIP implementation of the two clients
- Possibly generate a questionnaire to ask current core developers to know details regarding what they would like to see from a program like this

Second stage: More advanced form that is deployed somewhere (1-2 months)

- Get Granular code-level differences and store them in a cache based on sub-topics. Like it is done in RAG.
- Implement mechanism that will work fine even with longer context length. e.g. using cache as above or using layered approach: do 3 runs where each get progressively more detailed and finally combine them
- Explore various chunking and retrieval approaches that work best for our case. Simply semantic search won't work for complex code. 
- Support for multiple PRs of a client under a single EIP 
- Automated fetching of relevant PRs of a client rather than having to give exact PR link
- Dockerization 
- Research hosting options and build deployment guide

Third stage: Advanced repository level work & build from previous learnings

- Automated CI/CD pipeline
- Build finetuning pipeline to finetune a local LLM with latest code changes of a particular client repo (e.g. using LoRA)
- Experiment with agent frameworks (CrewAI, Autogen, LangGraph)
- ***Train a LoRA model with the past findings of security audit reports so that the program can learn from it***



## Possible challenges

`What are the limitations and issues you may need to overcome?`

The major challenge is the limited context length of llms which is at max 128K. And any code diff of even a mid level PR is more than 50K. So just the code diffs while comparing two PRs of two clients can well exceed the max context length. This is why the stage-wise summarization approach is necessary. But even then there might be challenges if the full diffs are really huge. In that case there needs to be mechanisms to ignore simpler changes and focus more on core changes to the code. e.g. 

## Goal of the project

`What does success look like? Describe the end goal of the project, scope, state and impact for the project to be considered finished and successful.`

End goad of this project will be a fully working webapp that has a frontend and backend hosted somewhere and it can create meaningful comparison reports for EIPs for multiple clients. 
Also a single docker-compose file that a developer can run locally. 

## Collaborators

### Fellows 

NA

### Mentors

Fredrik

## Resources

Provide links to repositories, PRs and other resources which constitute the project.

https://security.ethereum.org/wishlist
