# Niciam Dictatorship Constitution

## Good practices

- DRY: before adding new code spend some time looking at the existing code base, reuse as much as possible what's already there. https://www.jetbrains.com/help/resharper/dupFinder.html

- When implementing a new functionality for a specific customer always consider if it can benefit other cusomters as well and try to make the functionality as generic and resuable as possible, within reasons.

- If you need to work on a PBI which seems to span across multiple domains, before you jump into the implementation, go amd have a chat with other teams responsible for those other domains, they can provide you invaluable inputs and make your life easier.

- Every developer must reply "That's what she said" to any sort of innuendo, regardless to the seniority/gender of the people he/she is talking to.

## Coding Patterns

- Code against an interface when possible (Remeber to register your concrete implementation)
    - If the concrete implementation of your interface comes from an optional assembly, some customers may not be deployed with it. 
    An example being a customer that doesn't require billing may not have assmeblies containing Payment processors or Direct Debit information. 
    Consider using IOptional<T> unless you're certain it won't be an issue.

- Write unit tests for all the new code you commit
    - If altering legacy code, which isn't unit testible, think about creating a sprout class/method (http://xunitpatterns.com/Sprout%20Class.html)

- Keep your classes small and separate. We have too many huge files containing interfaces/concrete implementations and many many classes all in the same file which makes it really confusing to understand what's going on.

- Try not to return null. Returning null can lead to a world of hurt down the line. Consider using the Null Object pattern. (https://sourcemaking.com/design_patterns/null_object, https://en.wikipedia.org/wiki/Null_object_pattern#C#)

## Data interaction

- When using EF make sure you are writing queries using indexed properties
    - You can check indexes using sp_helpindex "Schema.TableName" or using SSMS

- If you need TasklistItem data, use ITasklistQueryBuilder interface.
    - When unit testing, use FakeTasklistQueryBuilder.

## Breaking changes

- When changing the signature of an existing method, make sure you think about the following:
    - Look for references to the method and check if and where the method is called from
    - If it's used in other places and you want to add a new parameter then give it a default value in order to keep the existing calling code working
    - If it's used in other places and you need to remove a parameter then make sure you refactor all the existing code appropriately
    - Make sure you haven't broken existing unit test and add new one to cover your new scenarios
    
- We're using a heavily event driven architecture, always think about "inflight" messages when making changes
    - If you change the name of an event, what's going to happen to the messages with the old event name?
    - If you add a new field, will the processing be able to cope with the inflight events without it?

## Pool Requests

- Try to explain what you're trying to achieve in the description of the PR, that will make the reviewer's life a lot easier and the code will be reviewed quicker when a clear context of what's going on is present

- If possible, keep your pull requests short and succinct. Trying to evaluate risk in a 300 file PR is near impossible.

- Add references to relevant PBIs

- Add Technical Tasks in case DB changes/Config changes are required

## Further reading

- https://sourcemaking.com/design_patterns