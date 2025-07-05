**Notice!!! this plugin is refactoring,the next update version might be very very different!!!**

# RGOAP Documentation

This is a simple plugin to let you use Goal Oriented Action Planning AI.Follow next steps to statrt build your own GOAP AI.



## First step:Know World State

First,know what is world state.world state is AI agent's subjective understanding of world.World state in RGOAP has two main elements,one is state's name,it's represent by gameplay tag,another one is a boolean.

## Second step:Goal Set

Create a new blueprint class based on GoalSet.


![image](https://github.com/user-attachments/assets/d17df796-197d-431a-8f35-bae2f23cb63a)


Add new goal,config Precondition,Desired State and priority


![image](https://github.com/user-attachments/assets/d225a90a-1fc9-4fbc-8317-0bf6ad1c8441)


Precondition is the precondition that decided if an agent can choose this goal as their task.
Desired state defined what kind world state this goal hope to be.For example,current state is "I am hungry" is true,and goal "happy live" wants the state become false.
Priority is basically the order of Precondition check.If one goal has been chose,we won't continue to check those goals below.

Notice it's better to set goals as a relatively broad goal.But also it's better to keep Desired world state simple enough that it won't take more than three Actions to achieve it.

Use GameplayTag define the state,and boolean to describe the state.


![image](https://github.com/user-attachments/assets/7c2273f4-9dd1-427d-87bc-b380e4543d87)


Set Desired world state,this is essentially goal itself.


![image](https://github.com/user-attachments/assets/899037a9-74d0-48dd-95a2-9d5d497f58e5)


set Priority to decide goal's check order.


![image](https://github.com/user-attachments/assets/c1f8d896-cab0-4618-b8ee-e12b82e5375d)


## Thrid step:Action

First, you create an Action,search “GOAPAction”,ad to define an action,it's the same you need to describe Precondition,when can this action be active.


![image](https://github.com/user-attachments/assets/2d2488c4-5fd6-4891-885f-9bb3f43a2904)


Effect of the action is the Effect when you apply this action.How you can change the current world state.


![image](https://github.com/user-attachments/assets/d664a9d7-d26e-48de-a59a-98565afdb31f)


Notice that this effect is not a real effect that can change the current world state.it's only theoratically what will happen when you successfully applied this action.

Cost is the price when you use this action.Cost is not connected to any of really exist sources in the game,you can think it's represent how AI reluctant to this action.For example,Fire without cover action's cost is higher than fire under cover.After all,seek benefits and avoid harm is a biological instinct. 


After you finish those works,let's define AI truely action.Search Event ExecuteAction in the graph.The out put pin is the GOAP Controller we are going to use,and the pawn that execute the action.

**Be sure to call ActionCompleted functio at the end of your logic!If it's not about any kind of successfully execute.just tick bWasSuccessful as true.**


![image](https://github.com/user-attachments/assets/f3d863f3-db47-4701-83e3-1b1130366221)


## Fourth Step:Config GoalSet and Actions

To defines which goal set we are using,we need to add a component to the pawn,search Actionset Component，add it to the pawn.


![image](https://github.com/user-attachments/assets/11e5c99e-5833-4524-8a65-0e203858db32)


click the component,under the category "GOAP",config goal set and add actions to the array.


![image](https://github.com/user-attachments/assets/398d1303-a16f-4fcf-9aad-9a0a853e8e47)


**See Actions as raw material,Goal as product plan,even you have a lot of plan ,if you lack of raw material, you still can't produce the product you want.Action is the "Ability of the body flesh",actions are relatively fixed,even you config same goal set as others,the behavior this agent shows may still different.**


## Fifth Step:Update World state

This is a flexible step, you can do whatever you want to update WorldState.if you want you cant create a function, call this update function every tick,or you can use behavior tree service to update,or you can use state tree task.In conclusion,you can did this as the way you like.

No matter how you update world state,be sure you are updating variable CurrentWorldState,you can get and set this variable as long as you have GOAP Controller.

There are two functions you can call to set and get AI itself world state.


![image](https://github.com/user-attachments/assets/6fe5d4bc-c580-4589-8812-274e599f62d6)


## Sixth Step:Use GOAPController

Search and create GOAPController blueprint to use on your ai pawn.


![image](https://github.com/user-attachments/assets/30a341cd-5abe-4cf6-b4c6-63284e9ef7eb)


A GOAPData Component has been created by default,here you can config a data asset called "memory".


![image](https://github.com/user-attachments/assets/9edd62ac-e1fa-4b20-aa7d-3e76f6abbe2a)


![image](https://github.com/user-attachments/assets/6c128322-cf31-4bb4-94d6-54082d786718)


Create data asset under the category "misc",choose data asset,choose GOAPMemory


![image](https://github.com/user-attachments/assets/3116dd61-03bc-4795-92d5-748e32199422)


![image](https://github.com/user-attachments/assets/e94d666c-ae31-466c-a81a-48bf191d4785)


Once you've done,click the memory,add initial world state in it,then config it in GOAPData component.

the final step is call TryExecutePlan every tick.


![image](https://github.com/user-attachments/assets/6ef3d053-4c6d-4e80-bffa-60d9e201481b)























