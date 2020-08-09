# Setting up Gulp 4
Modern web development has many repetitive tasks like running a local server, minifying code, optimizing images, preprocessing CSS and more. This text discusses gulp, a build tool for automating these tasks
## 1. Install NodeJS from the official site
`[Название ссылки](https://nodejs.org/en)`
## 2. Check for node, npm, and npx
`node --version`  
`npm --version`  
`npx --version`  
## 3. Install the Gulp command line utility
`npm install --global gulp-cli`
## 4. Create a new project
Create a project directory and navigate into it.
## 5. Create a package.json file in your project directory
`npm init`  
This will guide you through giving your project a name, version, description, etc.
## 6. Install the Gulp package in your devDependencies
`npm install --save-dev gulp`
## 7. Verify your gulp versions
`gulp --version`
## 8. Create a gulpfile
Using your text editor, create a file named gulpfile.js in your project root with these contents: