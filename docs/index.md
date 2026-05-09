**Notice!!! this plugin is refactoring,the next update version might be very very different!!!**

# RGOAP Documentation

This is a simple plugin to let you use Goal Oriented Action Planning AI.Follow next steps to statrt build your own GOAP AI.



## First step:Know World State

First, let's understand the "World State."
The World State represents an AI agent's subjective understanding of the environment. In RGOAP, a World State consists of two main elements:
Key: Represented by an FName.
Value: A boolean (True/False).
This FName is used to link the state with a Blackboard Asset.In the Blackboard, create a key of type 'Boolean'. Ensure that the Key Name is identical to the WorldState's FName.

## Second step:Goal Set

To direct the AI's behavior, you can create **GoalSet**. 
<img width="391" height="320" alt="image" src="https://github.com/user-attachments/assets/eadbad64-1080-40e2-a8e4-196801ffee4f" />


### GoalSet
A **GoalSet** is a reusable Data Asset, which means multiple characters can share the same set of goals. 

**How to create:**
1. Right-click in the **Content Browser**.
2. Navigate to **Miscellaneous > Data Asset**.
3. In the popup window, search for and select **AI Goal Set**.

<img width="352" height="301" alt="image" src="https://github.com/user-attachments/assets/4d27e60b-e568-4b37-a80f-e19c0ff9cee9" />


A Goal consists of two parts:
*   **Preconditions**: Requirements that must be met to start the goal.
*   **Desired World States**: The target state the AI wants to achieve.

> **Note on Priority:** In this plugin, priority is determined by the list order (top to bottom). The AI evaluates goals from the top down, so ensure your most important goals are at the top.

Precondition is the precondition that decided if an agent can choose this goal as their task.
Desired state defined what kind world state this goal hope to be.For example,current state is "I am hungry" is true,and goal "happy live" wants the state become false.

Notice it's better to set goals as a relatively broad goal.But also it's better to keep Desired world state simple enough that it won't take more than three Actions to achieve it.

Use FName define the state,and boolean to describe the state.


## Thrid step:Action

Action is a blueprint asset,search "AI Action" in blueprint class to create one.

<img width="460" height="382" alt="image" src="https://github.com/user-attachments/assets/62ca3d0b-7202-4f3d-9037-d418e4afe739" />


Actions are the actual behaviors performed after the decision-making process. Each Action consists of three key components: Preconditions, Effects, and Cost.
Preconditions: The specific requirements that the WorldState must meet for this action to be executable.
Effects: The predicted changes to the WorldState if the action is successfully completed.
Cost: The "weight" or effort required to perform the action. When multiple Actions can satisfy the Desired World State, the AI will prioritize the one with the lower Cost.Cost is not connected to any of really exist sources in the game,you can think it's represent how AI reluctant to this action.For example,Fire without cover action's cost is higher than fire under cover.After all,seek benefits and avoid harm is a biological instinct. 

<img width="362" height="287" alt="image" src="https://github.com/user-attachments/assets/51a664c1-ec2b-45c8-90d6-e068b6089265" />


> [!IMPORTANT]
> **Important Note:** The **Effect** is NOT the actual change that occurs in the game world. It exists only as a "mental reference" for the AI planner to figure out which steps are needed to achieve its goal.

<img width="352" height="295" alt="image" src="https://github.com/user-attachments/assets/f8b3f83c-f8cc-4072-b46a-6ef416cdd629" />


There are four functions you need to override/define. The first one is:
1. ActivateAction
This function handles the actual implementation of the Action's logic. Once the execution is complete, you must call the FinishExecuteAction function to return a success or failure status.
If successful: The AI will proceed to the next Action in the plan.
If unsuccessful: The current plan will be aborted, and the AI will initiate a re-planning process.
[1] [!WARNING]
Critical Timing: Calling FinishExecuteAction will cause the AI to transition to the next Action immediately. The system does not check if your current Action's internal processes (such as animations or timers) have actually finished. Ensure all necessary logic is fully complete before calling this function.

<img width="406" height="346" alt="image" src="https://github.com/user-attachments/assets/7ca757c5-97c3-489c-9b81-a6f7f9691264" />





<img width="305" height="275" alt="image" src="https://github.com/user-attachments/assets/7ee2969c-bd8d-486f-ae7f-3aa8c1ad04cf" />


2. DeactivateAction
This function is triggered both when an Action completes naturally and when it is interrupted/aborted. Its primary purpose is to handle cleanup logic.
Typical Use Case: Use this to stop playing Animation Montages, clear temporary timers, or reset variables to their default state to ensure the AI is ready for its next task.

<img width="358" height="278" alt="image" src="https://github.com/user-attachments/assets/eb910b2e-464d-4d74-b691-adeb2145cb09" />


3. EvaluateCost
By default, the Action Cost is set to -1.
When Cost is -1: The planner will automatically call the EvaluateCost function to determine the action's cost at runtime.
Purpose: This allows you to define dynamic costs based on the current situation (e.g., distance to a target or current health) instead of being limited to a fixed, static value. This makes your AI's decision-making much more flexible and "intelligent."

<img width="322" height="250" alt="image" src="https://github.com/user-attachments/assets/599c7493-0430-473e-bd02-0cebc81406a7" />


4. ValidateAction
This function is called frequently while the Action is active. It continuously monitors the environment to verify whether the requirements for executing the Action are still valid.
Functionality: If the situational conditions change and the Action can no longer be completed, this function ensures the AI can detect the failure immediately and stop the Action.

<img width="702" height="509" alt="image" src="https://github.com/user-attachments/assets/95f68f2a-a4a3-450d-95be-fb482442e316" />


That concludes everything you need to know about Actions.
Important: For Actions to function correctly, they must be included within an AI Action Set Data Asset. The system uses this asset to identify which actions are available for the AI to choose from.

## Fourth Step:Config GoalSet and Actions

<img width="325" height="227" alt="image" src="https://github.com/user-attachments/assets/17315653-0242-461b-b108-cba3adaec6d6" />


### Getting Started: Setup the Controller

To enable GOAP logic on your character, follow these steps:

1. **Create the AI Controller**: Create a new Blueprint class and search for **`ActionPlanningController`** as the parent class.
2. **Assign Assets**: In the **Details** panel, locate the **Action Planning** category.
3. **Link Your Assets**: Assign your custom **Blackboard**, **GoalSet**, and **ActionSet** to the respective slots.

Without these assignments, the AI will not have the necessary "knowledge" (Goals and Actions) to generate plans.

**See Actions as raw material,Goal as product plan,even you have a lot of plan ,if you lack of raw material, you still can't produce the product you want.Action is the "Ability of the body flesh",actions are relatively fixed,even you config same goal set as others,the behavior this agent shows may still different.**

The system is highly modular. For example, a Ninja has its own specific GoalSet and ActionSet, while a Rat has its own.
If you assign the Ninja's GoalSet to the Rat, the Rat will attempt to achieve those "Ninja goals" using its own available actions. Whether it succeeds depends on the Rat's capabilities; it may fail simply because it lacks the Ninja's physical attributes or specific skill set.
This separation allows you to mix and match goals and actions across different AI agents to create diverse and unpredictable behaviors.

<img width="368" height="292" alt="image" src="https://github.com/user-attachments/assets/45adb105-ee70-4406-8557-0766a71af9dc" />


To start the AI's logic, you need to call the StartActionPlanning function at the appropriate time (e.g., on BeginPlay or when a specific event is triggered). This will prompt the AI to begin evaluating its goals and generating plans based on the current WorldState.

## Fifth Step:Update World state

The system is designed to be non-intrusive. You have complete control over how and when the WorldState is updated.

#### How it works:
Because WorldStates are just **Blackboard Keys**, you can modify them from anywhere. Simply get a reference to your `AIController`, access its `Blackboard`, and use:
*   `SetValueAsBool`
*   `GetValueAsBool`

#### Possible Update Methods:
- **Behavior Trees/State Trees**: Use them to handle high-level logic and set keys.
- **Tick**: For states that need constant monitoring.
- **Event-Based**: Update states only when something specific happens (like "OnTakeDamage").

> [!CAUTION]
> **Naming is Critical:** The `FName` you use for a Precondition or Effect **must match the Blackboard Key name exactly**. If there is even a small typo, the GOAP planner will not be able to find the corresponding state.
























