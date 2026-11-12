I just had my first open source contribution merged. Working with an AI coding assistant, I added 25 automated tests to two helper functions in Elmo, an open source AI visibility project, and the maintainer approved and merged my pull request about two hours after I opened it. 🎉

The helpers build the link for the preview image that shows up when a page is shared, and nothing was testing them before. Tests are small programs that warn the next person who changes the code if something breaks.

I chose the issue, approved the plan and read every test before it went in. Before asking for a review:

✅ I had the assistant break the code on purpose to confirm the tests would notice, and they did.
✅ A checking tool from my course caught that my tests didn't follow the layout the maintainer asked for, so we fixed it before opening the pull request.

The maintainer then added a few test cases and changed how the code handles one edge case that the issue had told me to leave alone. That was his call, and a reminder that review improves the work.

Two takeaways: show your work before asking someone else to spend their time on it, and check that a problem isn't already solved before you start.

My pull request: https://github.com/elmohq/elmo/pull/1178

#OpenSource #SoftwareEngineering #Testing
