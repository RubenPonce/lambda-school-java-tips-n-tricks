# lambda-school-java-tips-n-tricks
A collection of tips for the Lambda School Java curriculum. This is meant to find little quips about a section or problem you may be currently dealing with specific to the curriculum, buildweeks, or your very own projects.

# Swagger-UI
Swagger UI comes out of the box with several features that aren't covered very in depth in the cirriculum. Navigate to `/swagger-ui.html`
 ### 1) Remove Configuration Controller documentation (to make nicer documentation for your Front End). 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Configuration Controllers include `basic-error-handler`, `web-mvc-handler`, just all these things your front end won't use in Build Week! To configure that, navigate to *Swagger2Config* in your config folder right after you instantiate the `Docker` class,replace `select().apis(RequestHandlerSelectors.any())` with `.select().apis(RequestHandlerSelectors.basePackage("com.yoursrc.yourpackage"))` and only the endpoints in your controllers will show up.
