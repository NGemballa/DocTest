System Scope and Context
========================

## Business Context

![03. Business Context](https://serviceware-plantuml.labs.sabio.de/png/PL71JW8n4BttAoPx1utU224WPoFH62vSnZBiDh3JjJCh4kE_EosW17jgN-UzUVDcAef9-tO7j_T3UEWbC8d0FBBIfpgFdOR8_p-SR0DtcB057zikCh6-w657O5ftpctiXw2wvbNmvS0EUeq9gbUXL04Az9Veh3Qn0gChU2AloBM9LukdvIoB53mVZck37wq7re7hGcjXF9Q3ABMJoLNX8aLljIIbiBik_7P7POJ1O0Bj29OnRGNsGHluxR5Yv3KWovwIhDZteh6b1EyzIKvBBYIw4SmJeHBSWCrFHkpE5bQyiN6riSmi1x2Oy_qD9GZ4JNdu1lNeg3yxcXjqSeGCcBEoW6fCQxC6viqMmfuIdsZZuJ3Jdia7-Y3vyornrnUxa6qqrlHCF7PiOLZiMx-SbQsxrrAgmMz9dMKrjNbitpy0 "03. Business Context")

**_User_**

The user interacts with Messaging via a chat widget on the self-service portal.

**_Agent_**

The agent can chat with the user via the embedded Messaging and tries to solve his concern. He can then create / extend a process via Processes 

**_Messaging_**

Messaging provides the chat as content for the new or extended process

**_Processes_**

Processes is in charge of displaying the processes and user information related to the user the chat is currently for. 

## Technical Context

![03. Technical Context](https://serviceware-plantuml.labs.sabio.de/png/RP1FImCn4CNl-HH3JlLIH6-bBDNYeGTfyGzUlCncfzlGpMH99c8HlxkpQwcYpIM4z-OtBs-H1PEKqC7bzHIUoEtOE-nW6LdeNOp3NdpaCuh9Uyyz9WpWOkWZ4ykZ73e2fLqPirM5mFuRjF1XG4yY9yYj-krc_N9ZOJvt9KiCb20Vib2eggFrZ_s1iF7SmWQPB626T7ATae2zfvon7NpFgz4LYgxmOwog6YS-pNYvBfP6KvIwymwMmykUt_p3l4Mt9sw-mVLlMgXXghjciDBl9YQ1oKwapnT7kQYSPd3tDgIiCLFpojfD26MeQIZRA6cXSEtn2m00 "03. Technical Context")

**_Messaging Backend_**

In the first version, the messaging backend is hosted in our public cloud infrastructure and connects via https to the customer system.

**_POWA_**

On the customer system, the new POWA client includes the messaging UI. Any communication with the parent context is based on events.

