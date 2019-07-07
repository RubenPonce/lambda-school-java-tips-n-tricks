# lambda-school-java-tips-n-tricks
A collection of tips for the Lambda School Java curriculum. This is meant to find little quips about a section or problem you may be currently dealing with specific to the curriculum, buildweeks, or your very own projects.


## Table of Contents
1. [Modifying application.properties](https://github.com/RubenPonce/lambda-school-java-tips-n-tricks#application-properties)
2. [Swagger-UI](https://github.com/RubenPonce/lambda-school-java-tips-n-tricks#swagger-ui)
3. [Repository Configurations](https://github.com/RubenPonce/lambda-school-java-tips-n-tricks#repository-configurations)
4. [Deploying to Heroku](https://github.com/RubenPonce/lambda-school-java-tips-n-tricks#depliying-to-heroku)


## Application Properties
Application properties can be changed in a number of ways. One way that is common in configuring them is to have your application properties split into groups. 

### Change your local configuration
`spring.profiles.active=local`can change your current configurations to a `application-local.properties`. This means you can have a configuration for *heroku*, a configuration for *testing*, and pretty much anything you can think of. So you don't have to go and comment out code anymore. 


## Swagger-UI
Swagger UI comes out of the box with several features that aren't covered very in depth in the cirriculum. Navigate to `/swagger-ui.html` to view the page. **Remember that all of this is optional**
 ### Remove Configuration Controller documentation (to make nicer documentation for your Front End). 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Configuration Controllers include `basic-error-handler`, `web-mvc-handler`, just all these things your front end won't use in Build Week! To configure that, navigate to *Swagger2Config* in your config folder right after you instantiate the `Docker` class,replace `select().apis(RequestHandlerSelectors.any())` with `.select().apis(RequestHandlerSelectors.basePackage("com.yoursrc.yourpackage"))` and only the endpoints in your controllers will show up.

### Manually Customize Your End Points 
Your End Point documentation can be customized to give a better look into what they do. The whole purpose of this documentation is to make it easier for the frontend to read what's going on in each controller and what to expect so. 

- *Name your own Controller Classes!* the `@Api` annotation allows you to name your controller instead of just giving the default! `%20` gives you spaces in the title of it. the description changes the 'subtitle' of your controller. And `produces` just tells you what your controller is going to deal with which will probably be JSON!
``` 
@Api(value = "User%20Request%20Options",produces = "MediaType.APPLICATION_JSON_VALUE", tags = {"users"},description =     "Where the magic for my users happen.")
@RestController
@RequestMapping(value = "/users")
public class UserController {... 
```

- *Name your indidvidual End Points inside your controller classes* The `@ApiOperation` notation gives you a description. But where you can really change things up is in what your endpoints will Respond with and what they mean. This will clear up a lot of confusion when your Front end is sending you Potato objects instead of User objects and wondering why your API doesn't work. Specify the Code, and put in the message. EZ Docs
``` 
@ApiOperation(value="creates a new user")
@ApiResponses(value = {
@ApiResponse(code= 201, message = "successfully created"),
@ApiResponse(code=400, message = "Bad Request, enter a proper JSON object")
})
@PostMapping(value = "/createnewproject", consumes = {"application/json"})...
```


## Repository Configurations
Sometimes you only want crud functionality in your repos, but you can also page your results with the Pageable class. 

### Configure Your Repos to use Pageables
The PagingAndSortingRepository extends the CRUD repository, so all methods in there will also be available in this interface.

**Repository**<br>
`YourClassRepository extends PagingAndSortingRepository<YourClass, Long>`

**Service**<br>
`List<YourClass> findAll(Pageable pageable);`

**ServiceImpl**<br>
```
public List<YourClass> findAll(Pageable pageable) {//ensure to add your pageable to all findAll methods 
        List<YourClass> list = new ArrayList<>();
        yourClassRepository.findAll(pageable).iterator().forEachRemaining(list::add);
        return list;
```
**and finally wherever you are returning JSON:** <br>
```
@GetMapping(value = "/yourclass", produces = {"application/json"})
    public ResponseEntity<?> findAllYourClassList(@PageableDefault(page=0, size = 3) Pageable pageable){
        return new ResponseEntity<>(yourClassService.findAll(pageable), HttpStatus.OK);
    }
```

## Deploying To Heroku
Instructions are curtesy of @Doc, and John Mitchell. In short, take a very long string from heroku that is gotten from first adding postgres to your heroku app, with `heroku addons:create heroku-postgresql -a yourApp` and then `heroku -a config` will return a very long url string that contains your username, password, databasename, and url for the database. 
for Example: <br>

**heroku config -a yourAPP** *DATABASE_URL:* postgres://ztukeavfelzbya:8f089d91e8987b1db6121784bb0b7ba3a8c3bbb232daaed88604e83f08edf819@ec2-54-227-251-33.compute-
1.amazonaws.com:5432/df41qidrqj2kbo<br>

**your username:** ztukeavfelzbya<br>

**your password:** 8f089d91e8987b1db6121784bb0b7ba3a8c3bbb232daaed88604e83f08edf819<br>

**your URL:** ec2-54-227-251-33.compute-1.amazonaws.com:5432/df41qidrqj2kbo<br>

**your Database name:** df41qidrqj2kbo<br>

```
Switch to postgres - change configs

Start in pom
   add postgres dependency to pom.xml

application.properties
    comment out h2 configuration

    add spring.datasource.url=jdbc:postgressql://localhost:5432/dbstarthere
        spring.datasource.username=postgresql
        spring.datasource.password=${MYDBPASSWORD}
        spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true

For for the first run,
    set - spring.jpa.hibernate.ddl-auto=create
          spring.datasource.initialization-mode=always

For the following runs,
    set - spring.jpa.hibernate.ddl-auto=none
          spring.datasource.initialization-mode=never

Note:  you need to set up an environment variable called MYDBPASSWORD that contains the password to your PostgreSQL installation.
       you need to create a database in PostgreSQL using pgAdmin called dbstarthere (or the name of your database)


H2ServerConfiguration
    comment out  @Configuration


SeedData
    comment out @Component


DEPLOY TO HEROKU

Create the application on Heroku
At the Command Line
    heroku login
         login to heroku at the CL

    browser popup
    enter username and password
    
    heroku create -a nameThatMatchesChoiceOnHeroku
        create application
        give name that matches pom.xml

    heroku addons:create heroku-postgresql -a nameThatMatchesChoiceOnHeroku
        add postgreSQL to your application

    heroku config -a nameThatMatchesChoiceOnHeroku at CL
        returns postgres URL

        postgres://rrwzjxlkniayov:83e8dc9dc5a3c3a30e40dde8fb62941da11030b3953709f5c8f808690e776c71@ec2-54-243-241-62.compute-1.amazonaws.com:5432/d7bl8dlv2l83jj    
        posgress://username      :password                                                        @url                                      :5432/dbname
    


application.properties

Edit:
    spring.datasource.url=${SPRING_DATA_URL:jdbc:postgresql://(find in CL return value everything after the @) '+this' &sslmode=require
    spring.datasource.username=(find in text of CL return value DATABASE_URL between // and :)
    spring.datasource.password=(find in text of CL return value DATABASE_URL between : and @)

    FINALLY add to spring.datasource.url
        after host name add ":port"
        and ?user='value'    copy&paste from spring.datasource.username
        and after user ?password='value'  copy&paste from spring.datasource.password

Check pgAdmin for results
    Create new Server - right click on "Servers"
        copy hostname from application.properties and paste into pgAdmin Server -> Create Server
        copy username from application.properties and paste into pgAdmin
        copy password from application.properties and paste into pgAdmin
        On Create - Server under Advanced tab add dbname

    Goto Query tab in pgAdmin and 'select * from sometablename'  to test


Back in IntelliJ
    Goto Maven tab
    click m
    CL enter clean heroku:deploy
    Click execute

Test with Postman
    get access token - use token (remember to change your access token URL to appname.herokuapp.com/oauth/token)

    GET
    http://appname.herokuapp.com/endpoint/endpoint

```
