<!--
.. title: Components, State machine, state, events, action
.. slug: components-state-machine-state-events-action
.. date: 2021-04-05 20:37:29 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

The world is a state machine driven by events

events are what components listening on

components responds to different events with different actions

different components cares only about events they listen on

state are what the actions condition on

some components share on some state while some do not

the actions of one components may affect the states of other components

in real world, all components runs on different thread, however it's not true in programming

in react, "state" are special state do something with ui behaviors, and can only be changed with setState, which can be changed immediately, but will be changed in order they been pushed into the queue(which is synchronous opeation)

in react, components are organized in a tree structure, each components is a node on the tree.

in react, each components have at least one action called render, which listen on render events.

![事件，状态，动作](/images/QQ截图20210405212650.png)
![共享状态-组件通信](/images/QQ截图20210405213404.png)
