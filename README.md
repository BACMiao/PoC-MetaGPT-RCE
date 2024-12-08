### Summary
While using the latest version (<=v0.8.1) of MetaGPT's `TreeofThought.solve` method, we discovered that users can guide the large language model through dialogue to generate malicious code. This code can then be executed by triggering the `eval` method within the `ThoughtSolverBase.generate_thoughts` function, enabling the execution of arbitrary commands. The risky code is shown in the figure below.

### Details
We used the template provided in [creative_writing.py](https://github.com/geekan/MetaGPT/blob/main/tests/metagpt/strategy/prompt_templates/creative_writing.py) and the test code in [test_creative_writing.py](https://github.com/geekan/MetaGPT/blob/main/tests/metagpt/strategy/examples/test_creative_writing.py) to validate that a malicious user can pass malicious code to the eval method in ThoughtSolverBase.generate_thoughts. By utilizing the jailbreak technique provided in the PoC, we bypassed the restrictions imposed by the creative_writing template on malicious user inputs. This allowed us to embed malicious code within the conversation, guide the large language model to return the malicious code, and subsequently trigger the RCE vulnerability.

### PoC
Please review the file tot_demo.py.

### Impact
The following diagram illustrates the process of executing the aforementioned code sample, which triggers the execution of malicious code. This code then reads relevant files from the server's local system (other actions, such as deleting files, can also be performed).

<img width="316" alt="image" src="https://github.com/user-attachments/assets/a8a24ccf-e521-4351-9604-38a668cbfbbb">

### Weaknesses
Improper Neutralization of Directives in Dynamically Evaluated Code ('Eval Injection') (CWE-95)

### Environment information
LLM type and model name: OpenAI gpt-3.5-turbo

System version: ubuntu18.04

Python version: python3.11

MetaGPT version or branch: 68b7dc6
