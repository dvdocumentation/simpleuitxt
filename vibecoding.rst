.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Generating solutions with neural networks
===========================================

SimpleUI solutions can be generated on prompt using LLMs such as ChatGPT, DeepSeek and others. You can create both entire configurations and parts, such as Processes. Only top models with a large number of parameters are suitable for generation. On junior models (of those that can be run locally) the result is much worse.

In order to generate and run a solution, you need to:

1. Go to the chat bot (for example https://chat.deepseek.com/) and start a new chat
2. Attach the LLM5.txt context file from the repository https://github.com/dvdocumentation/llm_context . *Some technologies (ActiveCV, dataset engine) are moved to separate files in the repository, if necessary, specify them as well.*
3. Write a request, giving precise instructions if possible.
4. Save the result in a .ui file (if you write "create a process" in the request, then you can simply copy the JSON process to the clipboard). Next, if this is a configuration, then you should open it in https://seditor.ru:1555/ **and be sure to click the "Save configuration" button after loading**. If this is a process, then "Paste" in the list of processes.
5. Check on the device. If necessary, repeat steps 3-5. Important: most likely, LLM will generate a new UID for you (unless otherwise specified in the request) and when loading (p. 4) there will be no publication UID (this is another UID, different from the configuration UID), so each time after loading you need to click "Save configuration"
