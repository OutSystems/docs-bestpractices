---
summary: OutSystems brings with it the ability to design and execute business processes with the Business Process Technology Add-on. This technical note describes the notation used for Process Modeling targeted at practitioners currently modeling processes using BPMN.
en_title: Notation Reference for BPMN Practitioners
guid: e7784786-6c20-4455-b5b1-26030914dd69
locale: en-us
app_type: traditional web apps, mobile apps, reactive web apps
platform-version: o11
---

# Notation Reference for BPMN Practitioners

OutSystems allows designing and executing business processes through the Business Process Technology Add-on. This technical note describes the notation used for Process Modeling targeted at practitioners currently modeling processes using BPMN.

| **BPMN 1.2** |**Description**|**OutSystems Business Process Technology**|**Description**|
|-------------|-----------|-------------|---------------|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_0.png)| Plain Start Event|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_1.jpg) |**Start**, initiates the process.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_2.png)| Plain End Event|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_3.jpg)|  **End**, finishes a process flow. Theprocess may continue in parallelflows.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_4.png)| Terminate Event|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_5.jpg)|  **End with Terminate property set to Yes**, ends all flows of the process.|                                                                           
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_6.png)| Start Event with a catching trigger: Message; Timer; Conditional; Signal or Multiple|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_7.jpg)| **Conditional Start**, initiates an alternative flow and may be triggered by a DB event or an explicit API call.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_8.png)|  Intermediate Event with a catching trigger: Message; Timer; Conditional; Signal or Multiple | ![image alt text](images/Notation-Reference-for-BPMN-Practitioners_9.jpg)| **Wait**, pauses the flow waiting for a timeout, a DB event, an API call or for a custom business logic returning true.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_10.jpg)| Intermediate Event with a throwing trigger: Message; Signal or Multiple|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_11.jpg)| **Automatic Activity**, can be used to broadcast an event (via DB) or call an API to deliver an event to a specific activity or process.                                                           |
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_12.png)| Task of type: Service, Script or Send (if not email)|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_13.jpg)| **Automatic Activity**, performs custom business logic in the application or in external systems.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_14.png)| Task of type: User or Manual|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_15.jpg)|  **Human Activity**, waits for a user or group to complete the given task.This activity shows up in the taskbox of one or more users.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_16.png)| Task of type: Receive|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_17.jpg)| **Wait**, for the Receive semantics the application should call the Close Wait API when a message is received.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_18.png)| Task of type: Send (for email)|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_19.jpg)| **Send Email**, sends a specified email message in the process flow.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_20.png)| Reusable Sub-Process|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_21.jpg)| **Execute Process**, runs a subprocess. The parent process only proceeds after all flows of the subprocess finish.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_22.png)| Exclusive Data-Based Decision (Gateway)|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_23.jpg)| **Decision**, directs to one outgoing gates based on custom business logic.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_24.png)|Exclusive Data-Based Decision (Gateway)|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_25.jpg)|  **Connector**, defines the execution order of activities in the flow.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_26.jpg)|Parallel Split (Fork)|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_27.jpg)| **Fork**, two or more outgoing connectors (except when starting from a Decision) divide the flow into parallel paths.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_28.jpg)| Fork and Join using a Parallel Gateway|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_29.png)| **Fork and Join**, is implemented by calling a sub-process with the two or more parallel paths that must be joined. The parent process only proceeds after all flows of the subprocess have ended.|
|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_30.png)| Text Annotation|![image alt text](images/Notation-Reference-for-BPMN-Practitioners_31.png)| **Comment**, can be used to annotate any element or area of the process model.|
