# RGOAP Documentation

This is a simple plugin to let you use Goal Oriented Action Planning AI.Follow next steps to statrt build your own GOAP AI.

## First step:Know World State

First,know what is world state.world state is AI agent's subjective understanding of world.World state in RGOAP has two main elements,one is state's name,it's represent by gameplay tag,another one is a boolean.

## Second step:Goal Set

Create a new blueprint class based on GoalSet.
//picture

Add new goal,config Precondition,Desired State and priority

//picture

Precondition is the precondition that decided if an agent can choose this goal as their task.
Desired state defined what kind world state this goal hope to be.For example,current state is "I am hungry" is true,and goal "happy live" wants the state become false.
Priority is basically the order of Precondition check.If one goal has been chose,we won't continue to check those goals below.

Notice it's better to set goals as a relatively broad goal.But also it's better to keep Desired world state simple enough that it won't take more than three Actions to achieve it.

Use GameplayTag define the state,and boolean to describe the state.

//pitcture

Set Desired world state,this is essentially goal itself.

//picture

set Priority to decide goal's check order.

//picture

## Thrid step:Action

To define an action,it's the same you need to describe Precondition,when can this action be active.

//picture

Effect of the action is the Effect when you apply this action.How you can change the current world state.

//picture

Notice that this effect is not a real effect that can change the current world state.it's only theoratically what will happen when you successfully applied this action.

Cost is the price when you use this action.Cost is not connected to any of really exist sources in the game,you can think it's represent how AI reluctant to this action.For example,Fire without cover action's cost is higher than fire under cover.After all,seek benefits and avoid harm is a biological instinct. 

//picture

After you finish those works,let's define AI truely action.Search Event ExecuteAction in the graph.The out put pin is the GOAP Controller we are going to use,and the pawn that execute the action.

**Be sure to call ActionCompleted functio at the end of your logic!If it's not about any kind of successfully execute.just tick bWasSuccessful as true.**

//Picture

## Fourth Step:Config GoalSet and Actions

To defines which goal set we are using,we need to add a component to the pawn,search Actionset Component，add it to the pawn.

//picture

click the component,under the category "GOAP",config goal set and add actions to the array.

//picture

**See Actions as raw material,Goal as product plan,even you have a lot of plan ,if you lack of raw material, you still can't produce the product you want.Action is the "Ability of the body flesh",actions are relatively fixed,even you config same goal set as others,the behavior this agent shows may still different.**

## Fifth Step:Update World state

This is a flexible step, you can do whatever you want to update WorldState.if you want you cant create a function, call this update function every tick,or you can use behavior tree service to update,or you can use state tree task.In conclusion,you can did this as the way you like.

//pitcture

No matter how you update world state,be sure you are updating variable CurrentWorldState,you can get and set this variable as long as you have GOAP Controller.

//picture

There are two functions you can call to set and get AI itself world state.

//picture


## Sixth Step:Use GOAPController

Search and create GOAPController blueprint to use on your ai pawn.

//picture

A GOAPData Component has been created previously,here you can config a data asset called "memory".

//picture

Create data asset under the category "misc",choose data asset,choose GOAPMemory

//picture

Once you've done,click the memory,add initial world state in it,then config it in GOAPData component.

//picture

the final step is call TryExecutePlan every tick.

//picture























