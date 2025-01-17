In this unit, you use developer tools to create the code for a starter web application.

## Create a new web project

::: zone pivot="csharp"

The heart of the .NET CLI tools is the `dotnet` command-line tool. Using this command, you create a new ASP.NET Core web project.

First, let's install the 8.0 version of `dotnet` into the Cloud Shell. For this exercise, we're using SDK version 3.1.102.

1. Download and install dotnet 8.0 by running the following commands in the Azure Cloud Shell window:

    ```bash
    wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
    chmod +x ./dotnet-install.sh
    ./dotnet-install.sh --channel 8.0
    export PATH="~/.dotnet:$PATH"
    echo "export PATH=~/.dotnet:\$PATH" >> ~/.bashrc
    ```

1. Next, run the following command to create a new ASP.NET Core Model-View Cotroller (MVC) application named "BestBikeApp":

    ```bash
    dotnet new mvc --name BestBikeApp
    ```

The command creates a new folder named "BestBikeApp" to hold your project.

### Optionally test your web app

Open a second command shell session by browsing to <https://shell.azure.com/> in a new tab. You can test your application locally on Azure. To do so, use the following steps:

1. From your primary command shell session, run the following commands to build and run your web application in the background:

    ```bash
    cd BestBikeApp
    dotnet run &
    ```

    You should get output like the following example:

    ```console
    [1] 1275
    <username> [ ~/BestBikeApp ]$ Building...
    warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
          No XML encryptor configured. Key {b4a2970c-215c-4eb2-92b4-c28d021158c6} may be persisted to storage in unencrypted form.
    info: Microsoft.Hosting.Lifetime[14]
          Now listening on: http://localhost:5176
    info: Microsoft.Hosting.Lifetime[0]
          Application started. Press Ctrl+C to shut down.
    info: Microsoft.Hosting.Lifetime[0]
          Hosting environment: Development
    info: Microsoft.Hosting.Lifetime[0]
          Content root path: /home/cephas_lin/BestBikeApp
    ```

    The output describes the situation after starting your app. In the previous example, the application is running and listening at port 5176, and the process number is 1275. Take note of the port on which your application is running and listening from your output.

1. In the same Cloud Shell session, run the following command to browse to your web application, replacing *5000* with the port you noted in the last step. For example:

    ```bash
    curl -kL http://localhost:5176/
    ```

    You should see some HTML appear, ending in the following lines:

    ```html
    <div class="text-center">
        <h1 class="display-4">Welcome</h1>
        <p>Learn about <a href="https://learn.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
    </div>
    
            </main>
        </div>
    
        <footer b-b5g3qljvtd class="border-top footer text-muted">
            <div b-b5g3qljvtd class="container">
                &copy; 2024 - BestBikeApp - <a href="/Home/Privacy">Privacy</a>
            </div>
        </footer>
        <script src="/lib/jquery/dist/jquery.min.js"></script>
        <script src="/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
        <script src="/js/site.js?v=hRQyftXiu1lLX2P9Ly9xa4gHJgLeR1uGN5qegUobtGo"></script>
        
    </body>
    </html>
    ```

1. Using the process number your previous output, stop dotnet. From the previous output example:

    ```azurecli-interactive
    kill 1275
    ```

::: zone-end

::: zone pivot="java"

To create a starter web application, we use Maven, a commonly used project-management and build tool for Java apps. We use the `maven-archetype-webapp` template to generate the code for our web application.

1. Run the following commands in Azure Cloud Shell now to create a new web app:

    ```bash
    cd ~
    mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DinteractiveMode=false -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.5
    ```

1. Now, run these commands to change to the new "helloworld" application directory and package the application for deployment:

    ```bash
    cd helloworld
    mvn package
    ```

1. When the command finishes running, run these commands to open the *target* directory and list its content:

    ```bash
    cd target
    ls
    ```

You now have a file called `helloworld.war` that's the web application package we're going to deploy to App Service.

::: zone-end

::: zone pivot="node"

To create a starter Node.js web application, we use Node Package Manager (`npm`) along with some JavaScript code to run the actual web page processing.

1. Run the following commands in the terminal to create a new `package.json` file that describes our Node.js application:

    ```bash
    cd ~
    mkdir helloworld
    cd helloworld
    npm init -y
    ```

A new `package.json` file is created in the current folder. You should find it in the current folder if you enter `ls` in the terminal window. We need a JavaScript file to run our website logic. For this basic example, we only need one file: `index.js`.

1. Create the file by running the following command in the terminal:

    ```bash
    touch index.js
    ```

1. Now we have to make a few edits to both of our files. To open an interactive editor, run the following command in the terminal:

    ```bash
    code .
    ```

1. To use Node.js to launch the web app, select the `package.json` file and make the following edits to the `scripts` section:

    ```json
    {
      "name": "helloworld",
      ...
      "scripts": {
        "start": "node index.js"
      },
      ...
    }
    ```

1. Save the file by selecting the **...** menu, or press <kbd>Ctrl+S</kbd> on Windows and Linux or <kbd>Command+S</kbd> on macOS.

    > [!IMPORTANT]
    > Whenever you edit a file in the editor, make sure to save afterwards by selecting the **...** menu in the top right corner of the editor, or by pressing <kbd>Ctrl+S</kbd> on Windows and Linux or <kbd>Command+S</kbd> on macOS. To exit the editor, press <kbd>Ctrl+Q</kbd> on Windows and Linux or click the top-right corner of the editor and select **Quit** on MacOS.

1. Switch to the `index.js` file, and add the following contents to it. This code is a small Node.js program that always responds with "Hello World!" when any GET request is made to the server.

    ```javascript
    const http = require('http');
    
    const server = http.createServer(function(request, response) {
        response.writeHead(200, { "Content-Type": "text/html" });
        response.end("<html><body><h1>Hello World!</h1></body></html>");
    });
    
    const port = process.env.PORT || 1337;
    server.listen(port);
    
    console.log(`Server running at http://localhost:${port}`);
    ```

1. Save your file and exit the editor by selecting the **...** icon on the top right and selecting **Save** and **Close Editor** from the context menu, or by using the accelerator keys <kbd>Ctrl+S</kbd> and <kbd>Ctrl+Q</kbd> on Windows and Linux.

### Optionally test your web app

While you're running the helloworld web app, you can verify it's running locally by opening a second command shell session.

1. From a new browser tab, browse to <https://shell.azure.com/>.

1. From your primary command shell session, run the following commands to start your web application:

    ```bash
    cd ~/helloworld
    npm start
    ```

1. From your second command shell session, run the following command to browse to your web application:

    ```bash
    curl http://127.0.0.1:1337/
    ```

    You should get the following output:

    ```html
    <html><body><h1>Hello World!</h1></body></html>
    ```

1. From your primary command shell session, press <kbd>Ctrl+C</kbd> to quit your web app, then close the second shell session browser tab.

::: zone-end

::: zone pivot="python"

To create a starter web application, we use the Flask web-application framework.

1. Run the following commands in Azure Cloud Shell to set up a virtual environment and install Flask in your profile:

    ```bash
    python3 -m venv venv
    source venv/bin/activate
    pip install flask
    ```

1. Run these commands to create and switch to your new web app directory:

    ```bash
    mkdir ~/BestBikeApp
    cd ~/BestBikeApp
    ```

1. Create a new file for your web app by opening *application.py* in the python interactive editor:

    ```bash
    code application.py
    ```

1. Copy and paste the following Python code to create the main web app functionality:

    ```python
    from flask import Flask
    app = Flask(__name__)
    
    @app.route("/")
    def hello():
        return "<html><body><h1>Hello Best Bike App!</h1></body></html>\n"
    ```

1. Save your file and exit the editor by selecting the **...** menu on the top right, and then selecting **Save** > **Close Editor**, or by pressing <kbd>Ctrl+S</kbd> and <kbd>Ctrl+Q</kbd> on Windows and Linux.

1. To deploy your application to Azure, you need to save the list of application requirements you made for it in a *requirements.txt* file. To do so, run the following command:

    ```bash
    pip freeze > requirements.txt
    ```

### Optionally test your web app

You can test your application locally in Azure while it's running.

1. Open a second command shell session in a new browser tab  <https://shell.azure.com/>.

1. From your primary command shell session (in the right-hand window), run the following commands to activate the virtual environment:

    ```bash
    cd ..
    source venv/bin/activate
    ``` 

1. From your primary command shell session (in the right-hand window), run the following commands to start your web application:

    ```bash
    cd ~/BestBikeApp
    export FLASK_APP=application.py
    flask run
    ```

1. From your second command shell session, run the following command to browse to your web application:

    ```bash
    curl http://127.0.0.1:5000/
    ```

    You should get the following HTML output:

    ```html
    <html><body><h1>Hello Best Bike App!</h1></body></html>
    ```

1. From your primary command shell session, press <kbd>Ctrl+C</kbd> to quit your web app, then close the secondary Azure Cloud Shell.

::: zone-end
