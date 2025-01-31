# Overview
Microsoft's Power Virtual Agent allows you create powerful bots that can answer questions in seconds without the help of data scientists or developers. You can decide to create your bot and integrate as a standalone web application, share on Microsoft Teams, on your website, on cortana, telegram, slack or kik.

Using a Power Automate flow connected to your bot, you can collect responses from your users, collate and upload those responses on SharePoint. Love to see how to make this happen? Let's begin!

# Prerequisites
Before we begin designing our bot, let's take some time to talk about how it is expected to work. 

Since our bot is designed for the survey only, our employees would need to start off the conversation with related phrases. These phrases are take survey, employee survey, start survey, survey, and begin survey. We would be using these as our trigger phrases for now.

The bot thanks the employee for taking the survey and asks for their department. It asks the survey questions and collects the employee's rating per question. At the end of the survey, each employee's response is collated and updated on SharePoint in real-time.

# Building your Power Virtual Agents Bot
Head over to http://aka.ms/TryPVA and sign in with your Microsoft account. Once signed in, you are directed to a page that looks like the image below.

![](/Images/powervirtualagents-1.PNG)

First give your bot a name. I would name mine **SurveySolution**. Then select your preferred language. Next you are expected to select an environment. I would use my default environment because I created my Power Apps application in that environment. An environment is a space where your organization can store, manage, and share business data, apps, and flows. After filling in the details, click 'Create'. You would be directed to an interface that looks exactly like the one in the image below.

![](/Images/powervirtualagents-2.PNG)

On the right navigation bar, click on 'Topics'. The next page you are directed to has 12 existing topics. 

A topic defines a how a bot conversation plays out. In this sample, we would create new topics from scratch. Topics have trigger phrases — these could be phrases, keywords, or questions that a user is likely to type. We also have conversation nodes — these are what you use to define how a bot should respond and what it should do.

![](/Images/powervirtualagents-3.PNG)

Delete the first 4 topics. To do so, open each topic and click on 'Delete' at the top right of the page. After deleting the topics, you would be left with 8 existing topics.

Now let's create a new topic for the purpose of our survey. On that same page, click on '+ New topic' at the top. Give your topic a name. I would name mine "TakeASurvey". 

In the section 'Trigger Phrases', I would input up to 5 phrases our employees would possibly type in when about to start the survey.

![](/Images/powervirtualagents-4.PNG)

Click on 'Go to authoring canvas'. The authoring canvas is where you define the bot's conversation pattern. You would notice that a blank Message node was automatically added. In the empty box, input your welcome message. Mine is below.
``` Text
Thank you for taking out time to respond to this survey. We totally appreciate you!
The goal of this short survey is to ensure that you are enjoying being part of our organization. Please fill only once!
```

Next, add another node by clicking the '+' icon and the 'Show a message'. We need to add a message telling the employee a little bit about the survey. Notice that a new empty text box is added to the flow. In that box, I would input the details below.
``` Text
This survey consist of 4 questions.
For each question, you are expected to tell us how you feel. 0 stands for lowest and 5 for highest.
Be assured that your anonymous responses would go a long way in making our organization better.
```

Before asking our questions, we need to identify which department the employee works at. This detail would go along way in helping us position our organization properly. To add this question, click on the '+' icon again, and this time click on 'Ask a question'. Notice that as you do, a different type of block is added to the flow.

Fill in the details correctly.
* *Ask a question:* The question we need an answer to is "Which department do you work with?", so we would input it in that box.
* *Identity:* This is a direct question, so we would pick the employee's entire response.
* *Save response as:* It is important we rename our bot variable for each question, so it's easy to reuse. Click on the pencil icon and in the column that pops out by the side, change the bot variable name for this question to ***department***. Bot variables are global variables as they are used to store information that do not change from topic to topic.

![](/Images/powervirtualagents-5.PNG)

Next, we would need to add each question and the options. We repeat the steps above. Click on the '+' icon again, and click on 'Ask a question'.

Fill in the details of the block correctly.
* *Ask a question:* Our first question is "Do you feel good about your current work/life balance?", so we would input it in that box.
* *Identity:* Unlike the question about department, this is a multiple choice question as participants have to select from 0 to 5.
* *Options for user:* Input the different options. In our case, it would be 0 to 5.
* *Save response as:* Change the bot variable name for the first question to ***question1***.

Notice that beneath the question block, multiple condition blocks have been added. A separate condition block was created for each option as shown in the image below.

![](/Images/powervirtualagents-6.PNG)

After collecting the response to that question, the bot is expected to ask the second question. To make this happen, we need to add a new node to one of the conditions. Click on the '+' sign and then 'Ask a question'. Fill in the block correctly. For 'Ask a question', input the next question. 'Identity' and 'Options for user' is the same throughout as above. For 'Save response as', give each question a new variable name - ***question2*** for the second question, ***question3*** for the third question and ***question4*** for the fourth question.

Now we need to connect all condition blocks earlier to the second question. We do this so that whatever the response to the first question is, the second question would still be asked. To do this, click on the '+' sign and instead of adding a node, click on the small circle and connect to the second question block. Repeat this process for all conditions. Your flow should look like the image below.

![](/Images/powervirtualagents-7.PNG)

Repeat this process for questions 2 and 3 - question 2 connecting to question 3 and question 3 connecting to question 4. After the 4th question, our bot should thank the employee for taking out time to fill the survey. To make this happen, add a 'Show a message' node to the flow, type in a short thank you message, and connect all conditions to the message as shown in the image below.

![](/Images/powervirtualagents-8.PNG)

You can test your bot to make sure it's working well by clicking 'Test your bot' on the left side of the page.

# Collecting Employee's Responses and Updating on SharePoint with Power Automate
CONGRATULATIONS ON SUCCESSFULLY BUILDING YOUR BOT! Now let's connect our bot to our SharePoint using Power Automate.

We start off by adding a new node to our bot after the thank you message. Remember how to add a node? Just click on the '+' sign. This time we would pick 'Call an action' then 'Create a flow'. Clicking 'Create a flow' would open up Power Automate in a new window as shown below.

![](/Images/powerautomate-1.PNG)

Notice that two actions have been automatically added to our Power Automate flow. 

In the first action, Power Automate is establishing a connection with our Power Virtual Agent bot. We need to add each question the bot would ask or we would need response to as an input. 

To add the questions as inputs, click on '+ Add an input', then 'Text'. Notice that when you do, a new input box is added. Repeat this step for all the questions and fill in the details correctly.
* *Input:* Use the same variable name you assigned to each question in the bot as the name of the input. For example, Department for the question to identify which department in your organization the employee works with, Question1 for the first question, Question2 for the second question, Question3 for the third question, and Question4 for the fourth question.
* *Please enter your input:* Enter each question as input. Ensure you input the questions as arranged on SharePoint and in your Power Virtual Agent bot.

![](/Images/powerautomate-2.PNG)

We would head over to our Power Virtual Agent bot now and beneath the last message, we would add a new node but this time a 'Call an action'. You would notice that a new flow has been added to the list "Power Virtual Agents Flow Template". Select it. Immediately you do, you would notice the flow opens up. Something like the image below.

![](/Images/powerautomate-3.PNG)

Now we need to select a variable name for each input. Remember we said our variable names would be used later to identify inputs? Now it's time to make use of them. Select the variable name that matches each question as shown below.
* Department(text) would get it's value from variable *department*,
* Question1(text) would get it's value from variable *question1*,
* Question2(text) would get it's value from variable *question2*,
* Question3(text) would get it's value from variable *question3*,
* Question4(text) would get it's value from variable *question4*.

![](/Images/powerautomate-4.PNG)

The second action automatically added to our Power Automate flow returns the values of the first action to Power Virtual Agents. We would need to create an output for each input entered in the first action. Repeat this step for all the inputs and fill in the details correctly.
* *Enter title:* Enter each question as input. Ensure you input the questions as arranged on SharePoint and in your Power Virtual Agent bot.
* *Enter a value to respond:* Click on 'Add dynamic content' and the pop up, select the variable name for each question.

![](/Images/powerautomate-5.PNG)

Our Power Virtual Agent flow would collect the response from the bot using the variable name assigned and use it as an output which would be used to create an item on our SharePoint list.

To automatically create an item on our SharePoint list, we would need to add a new action to our Power Automate flow. Head over to Power Automate and click '+ New step', search for "Create item (SharePoint)" and select. If you haven't already, you would need to sign in to SharePoint to create a connection.

Click on 'Site Address' and select the SharePoint site you created your SharePoint list in. Click on 'List Name' and select the specific SharePoint list you created for this solution. Fill in correctly the Power Virtual Agent output that you want each column of your SharePoint List to work with.
* *Title:* Click on 'Add dynamic content' and select the variable name we used to store the question "Which department do you work with?" - *department*. This would pick the response from that question and add it to that column.
* *Do you feel good about your current work/life balance?:* In our bot, this was the first question. To add this as an output, click on the textbox, then 'Enter custom value'. Click  the dynamic content *question1*
* *Do you feel like a true part of your team?:* Click on the textbox then 'Enter custom value'. Select the variable *question2*
* *Do you feel like you are heading in the right direction as a team?:* Follow the same process above, but this time select the dynamic content *question3*
* *Do you feel energized about your job?:* Follow the same process above, but this time select the dynamic content *question4*

At the end, your flow should look like the image below.

![](/Images/powerautomate-6.PNG)

Your SharePoint List would check for the output to those questions gotten from the bot and then update it to the specified column. Now save your Power Automate flow and your Power Virtual Agents bot and test the bot again.

When you completely respond to all questions and have received the thank you message, you would notice that a new response would be automatically added to SharePoint.

HOLLA! 💃🕺✨✨ CONGRATULATIONS ON REACHING THIS MILESTONE! 

You can now publish your Power Virtual Agent bot by clicking 'Publish' in the left navbar. Then in the page that opens up, click Publish.

We would need to share our Power Virtual Agent bot on Microsoft Teams so our employee can access it. To do so, head over to 'Manage' and click on 'Channels'. In the new page that opens up, click on 'Microsoft Teams'. Your page should look like the one in the image below.

![](/Images/powervirtualagents-9.PNG)

Click on 'Turn on Teams' so your bot would be turned on. To give everyone in your organization access to the bot, click on 'Availability options'. Select 'Show to everyone in my org' and 'Submit for admin approval' then 'Yes' to complete submission to administators of your organization's Microsoft Teams if you are not one.

![](/Images/powervirtualagents-10.PNG)

If you are admin in your organization already, head over to [Microsoft Teams Admin Center](https://admin.teams.microsoft.com/dashboard) to activate the submitted bot. Click on 'Teams apps' then 'Manage apps' in the left navbar. Your screen should look like the one below.

![](/Images/powervirtualagents-11.PNG)

Notice that there is 1 submitted custom app pending approval. Search for the name you used when saving your PVA bot. I saved mine as "SurveySolution" so I would search for it using that name. Click on "SurveySolution" when the name pops up.

![](/Images/powervirtualagents-12.PNG)

Click on the dropdown in "Publishing status" and switch status from 'Submitted' to 'Publish', then click 'Publish'. 

When published, your employees can head over to the 'Apps' section of Microsoft Teams under "Built for your org", find the bot and add.

In the section [Scheduled Reminder with Adaptive Card](/Scheduled%20Reminder%20with%20Adaptive%20Card/README.md) I would show you how to send out scheduled reminders to your employees when it's time to take the survey.

To learn how to visualize data stored on SharePoint, check out [Visualizing your Data with Power BI](/Visualizing%20your%20Data%20with%20Power%20BI/README.md).