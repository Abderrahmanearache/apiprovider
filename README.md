<h1>Welcome to API Provider
</h1>
<hr>

<h3>Download</h3>
<p>
    If you would like to taste API Provider <a href="https://github.com/dev-hb/apiprovider" target="_blank">git it from Github</a>.<br />
    <i>Don't forget to give us a star &star;</i>
</p>

<h3>API Provider command line file</h3>
<h4>Description</h4>
<p>
    This file helps you to create models easily without writing a single line of code, that helps also to build a well formed
    model that should not have problems during the api process.
</p>
<h4>Usage</h4>
<p>
    To use this file, enter to command line and switch to project directory then execute <code>php provider [OPTION [VALUE]]</code>
    Valid options are :
    <ul>
        <li><code>create_model Modelname</code> : This one creates a model with the given name</li>
        <li><code>create_preferred Modelname</code> : Create a preferred model under preferred directory</li>
        <li><code>clear</code> : This command deletes all models in project (critical usage)</li>
        <li><code>grant</code> : Used to give a model the permission to select, update, insert, delete or all</li>
        <li><code>revoke</code> : Used to delete model's permissions</li>
        <li><code>update</code> : Update the API Provider to a higher version is exists (not available yet)</li>
    </ul>
    Example of use : php provider create_model Product<br />
    This will ask you fields of the model, the fields names <b>MUST</b> be the same in the database table, if you are done with fields
    then just let the next one empty and press <code>ENTER</code> to finish the process and you will get your model ready to be used.
</p>

<h3>API Provider CRUD</h3>
<h4>Description</h4>
<p>
    This API Provider comes with a built-in CRUD url that reduces you efforts to about 60%
</p>
<h4>Usage</h4>
<p>
    To use the API Provider API you will have first to configure the <code>.connection</code> file that contains 4 global variables to
    be able to connect to MySQL database.<br />
    File schema :<br />
    <ul>
        <li>hostname : usually localhost or alternatively 127.0.0.1</li>
    <li>username : MySQL database username (default is root)</li>
    <li>password : Your database password</li>
    <li>dbname : database name</li>
</ul>
<i>Ps : the file must not have white spaces ot empty lines</i>
<br>
    To use CRUD links follow this link structure :<br />
    <code>
        hostname/project_name/index.php?context=[CONTEXT]&action=[ACTION][&FIELD=VALUE[...]]
    </code>
    The context
</p>

<h4>Valid CRUD actions</h4>
<p>
    The valid CRUD actions that are built with API Provider are : <br />
    <ul>
        <li><code>findAll</code> : give you the possibility to fetch all rows from a table</li>
        <li><code>find&[column=?&...]</code> : retrieve rows where a condition</li>
        <li><code>count</code> : counts total rows</li>
        <li><code>count&[column=?&...]</code> : counts rows where a condition</li>
        <li><code>delete&column=?</code> : delete rows where a condition</li>
        <li><code>insert&[column=?&...]</code> : inserts a row with given fields</li>
        <li><code>update&condition=?&[column=?&...]</code> : updates a row's fields with given condition</li>
    </ul>

<i>For the update link the first param after action MUST be the conditional row such us ID then followed by other fields</i> 
</p>

<h4>Examples (project devcrawlers.com/api)</h4>
<p>
    Select all products : <code>https://devcrawlers.com/api/index.php?context=product&action=findAll</code><br/>
    Select a product with ID 7 : <code>https://devcrawlers.com/api/index.php?context=product&action=find&id_pro=7</code><br/>
    Select all products cost 100$ : <code>https://devcrawlers.com/api/index.php?context=product&action=find&price=100</code><br/>
    Delete a product : <code>https://devcrawlers.com/api/index.php?context=product&action=delete&id_pro=2</code><br/>
    Count products : <code>https://devcrawlers.com/api/index.php?context=product&action=count</code><br/>
    Count products cost 100$ : <code>https://devcrawlers.com/api/index.php?context=product&action=count&price=100</code><br/>
    Update a product with ID 7 : <code>https://devcrawlers.com/api/index.php?context=product&action=update&id=7&price=160</code><br />
    Insert a new product : <code>https://devcrawlers.com/api/index.php?context=product&action=insert&name=toto&price=100</code><br />
</p>

<h3>Using Preferred Handler</h3>
<h4>Description</h4>
<p>
    The preferred handlers gives you the possibility to perform complex queries with a single line of code
</p>
<h4>Usage</h4>
<p>
    Use the provider command line to generate a custom handler then implement handle function using cases from given action<br />
</p>
<h4>Example</h4>
<p>
    Creating a query to retrieve all products and their categories, here is how the file must look like<br />
    <code>
        &lt?php

        class Mypreferredmodel extends PreferredHandler {
    
            public function handle(){
                // TODO add actions to take here (Handle both GET and POST methods)
                // Create Dracula instance and execute the query method
                 switch ($this->getAction()){
    
                     case 'find':
                         (new Logger())->json((new Dracula())->query("SELECT * FROM product NATURAL JOIN category"));
                         break;
    
                }
            }
    
        }
    
        ?>
   </code>
<p>
    By accessing the 'find' action you will get the json response.<br />
    <code>https://devcrawlers.com/api/index.php?context=mypreferredmodel&action=find</code>
</p>
<br />
<br />
Now that you are able to create a preferred model, lets discover what it offers you : <br />
<ul>
    <li><code>$this->getAction()</code> : retrieves the action provided in the link</li>
    <li><code>$this->getParams()</code> : returns all parameters passed in the link</li>
    <li><code>$this->getDirectory()</code> : returns the current working directory (default is preferred)</li>
    <li><code>$this->getDracula()</code> : return Dracula object to perform MySQL requests (query() & queryUpdate())</li>
    <li><code>$this->getContext()</code> : return current context (the name of preferred model)</li>
    <li><code>$this->getMethod()</code> : return request method (GET or POST)</li>
</ul>
</p>

<h3>How to use this provider with Javascript</h3>

<p>
    You can use this code with Javascript frameworks too such as ReactJS, VueJS, React Native, Angular ...
</p>

<h4>Using GET request</h4>

<p>
    This options is the best way to retrieve find and findAll actions

        fetch(url_to_api_provider)
        .then(response => response.json())
        .then(data => {
        // data contains you json response
        })
        .catch(err => console.log(err))
</p>

<h4>Using POST request</h4>

<p>
    To ensure that your sensitive data are transferred securely for insert, update and delete actions use POST request

        fetch(url_to_api_provider,
            {
            method: "POST",
            headers: {
                'Accept': 'application/json, text/plain, */*',
                "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
            },
                body: "context=produit&action=insert&id=1137&name=Product2&price=160",
            }
        )
        .then(response => response.json())
        .then(data => {
            // data contains you json response
        })
        .catch(err => console.log(err))
</p>


</p>

<h3>WebGet context</h3>
<h4 id="webget">Description</h4>

<p>
    This context gives you the possibility to fetch a url and retrieve its source code.
</p>

<h4>Usage</h4>
<p>
    To use this context follow the following link structure :
    <code>hostname/project_name/?context=webget&url=?</code>
</p>

<h4>Example</h4>

<p>
    Get the source code of DevCrawlers home page :<br />
    <code>hostname/project_name/?context=webget&url=https://devcrawlers.com</code>
</p>


<hr>
2020 &copy; Crawlers Open Source, All Rights Reserved to <a href="https://devcrawlers.com" target="_blank">DevCrawlers</a>