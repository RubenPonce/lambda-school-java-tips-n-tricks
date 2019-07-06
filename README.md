# lambda-school-java-tips-n-tricks
A collection of tips for the Lambda School Java curriculum. This is meant to find little quips about a section or problem you may be currently dealing with specific to the curriculum, buildweeks, or your very own projects.

## Swagger-UI
Swagger UI comes out of the box with several features that aren't covered very in depth in the cirriculum. Navigate to `/swagger-ui.html` to view the page. **Remember that all of this is optional**
 ### Remove Configuration Controller documentation (to make nicer documentation for your Front End). 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Configuration Controllers include `basic-error-handler`, `web-mvc-handler`, just all these things your front end won't use in Build Week! To configure that, navigate to *Swagger2Config* in your config folder right after you instantiate the `Docker` class,replace `select().apis(RequestHandlerSelectors.any())` with `.select().apis(RequestHandlerSelectors.basePackage("com.yoursrc.yourpackage"))` and only the endpoints in your controllers will show up.

### Manually Customize Your End Points 
Your End Point documentation can be customized to give a better look into what they do. The whole purpose of this documentation is to make it easier for the frontend to read what's going on in each controller and what to expect so. 

- *Name your own Controller Classes!* the `@Api` annotation allows you to name your controller instead of just giving the default! `%20` gives you spaces in the title of it. the description changes the 'subtitle' of your controller. And `produces` just tells you what your controller is going to deal with which will probably be JSON!
`
@Api(value = "User%20Request%20Options",produces = "MediaType.APPLICATION_JSON_VALUE", tags = {"users"},description = "Where the magic for my users happen.")
@RestController
@RequestMapping(value = "/users")
public class UserController {...`

- *Name your indidvidual End Points inside your controller classes* The `@ApiOperation` notation gives you a description. But where you can really change things up is in what your endpoints will Respond with and what they mean. This will clear up a lot of confusion when your Front end is sending you Potato objects instead of User objects and wondering why your API doesn't work. Specify the Code, and put in the message. EZ Docs
``` @ApiOperation(value="creates a new user")
    @ApiResponses(value = {
    @ApiResponse(code= 201, message = "successfully created"),
    @ApiResponse(code=400, message = "Bad Request, enter a proper JSON object")
    })
    @PostMapping(value = "/createnewproject", consumes = {"application/json"})...```
