https://www.linkedin.com/posts/nk-systemdesign-one_most-people-think-concurrency-parallelism-activity-7341068418319589376-ZRCy?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAADljAVIBBJU8f7HRFRTUnUC5fvHC3DaDx_U

![image (1)](https://github.com/user-attachments/assets/afa6952a-ea22-4f6f-ad29-d06c76704a89)
![task_decomposition](https://github.com/user-attachments/assets/1e2bc9d4-9736-46d5-96ff-2748eb59d093)

![1750242533070](https://github.com/user-attachments/assets/e02a3575-dc6a-4cb7-adc0-9d48a98a471c)


```
import autogen

config_list = [
    {
        'model': 'gpt-4',
        'api_key': 'API_KEY'
    }
]

llm_config={
    "request_timeout": 600,
    "seed": 42,
    "config_list": config_list,
    "temperature": 0
}

assistant = autogen.AssistantAgent(
    name="CTO",
    llm_config=llm_config,
    system_message="Chief technical officer of a tech company"
)

user_proxy = autogen.UserProxyAgent(
    name="user_proxy",
    human_input_mode="NEVER",
    max_consecutive_auto_reply=10,
    is_termination_msg=lambda x: x.get("content", "").rstrip().endswith("TERMINATE"),
    code_execution_config={"work_dir": "web"},
    llm_config=llm_config,
    system_message="""Reply TERMINATE if the task has been solved at full satisfaction.
Otherwise, reply CONTINUE, or the reason why the task is not solved yet."""
)

task = """
Write python code to output numbers 1 to 100, and then store the code in a file
"""

user_proxy.initiate_chat(
    assistant,
    message=task
)

task2 = """
Change the code in the file you just created to instead output numbers 1 to 200
"""

user_proxy.initiate_chat(
    assistant,
    message=task2
)

```
