# Part 1 - Project Setup

## The frontend application (Vuejs/NUXT.js) 
After you have installed all of the prerequisites.

Open a terminal/console window and navigate to the folder where you want to create you project. For example if you keep your source code in a folder called 'Code' then on windows computer this could be:

    cd C:\Code

or on a Mac this could be

    cd ~/Documents/Code/

Now create a  folder for our project:

    mkdir TechOnTheTyne-TutorialHandsOnWithJS

Enter the newly created folder.

    cd TechOnTheTyne-TutorialHandsOnWithJS

Now lets create our Nuxt.js  application and lets call it 'frontend'. This will be the interface for our end users.

    yarn create nuxt-app frontend  

After running the command above, you will be asked a number of questions, but you only need to answer the following question. Otherwise, just hit enter:

*Note: the frontend application will use the Yarn package manager*

    ? Project name: frontend
    ? version: 1.0.0
    ? Project description: TechOnTheTyne Tutorial
    ? Choose the package manager: Yarn
    ? Choose UI framework: None
    ? Choose custom server framework: None (Recommended)
    ? Choose Nuxt.js modules: ◯ Axios  (press space to select)
    ? Choose linting tools: None
    ? Choose test framework: None
    ? Choose rendering mode Universal: Universal (SSR)
    ? Choose development tools: 
    ? jsconfig.json (Recommended for VS Code)  (press space to select)


Now lets run our frontend application to ensure that all is working as expected.
In you terminal enter the following commands:

    cd frontend
	yarn dev

The application should now be running at the following address:

    http://localhost:3000
