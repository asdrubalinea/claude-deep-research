# Initial Request

**Timestamp:** 2025-07-06 14:07  
**User Request:**

I would like to modify the general file structure of the Deep Research functionality. Currently, when you start a new Deep Research, everything is saved in the current directory, in the session directory. What I want, instead, is that the user creates a new folder for each of their research, and then, all the data will be stored in that directory. This means that we have a directory per research, the user can include as many sources as they want in that directory (which will then be used by Deep Research), and they can also start multiple "similar" researches on the same dataset, reusing the previously gathered sources.

## Key Points Identified:
1. Current behavior: Deep Research saves to `sessions/` directory in current working directory
2. Desired behavior: User creates a dedicated folder per research topic/project
3. Sources should be stored in and reused from the research folder
4. Multiple research sessions can share the same source dataset
5. User can manually add sources to the research folder for Deep Research to use