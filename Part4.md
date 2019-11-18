# Part 4 - CMS Model Creation

## Define the 'TakeAways' Content Type

In Strapi a Content Type, (also called a model), is used to define the structure of our data.  We need to create our 'takeaways'  Content Type which we will call 'takeaway' note that we use a non plural name for out models name.

In the admin interface for Strapi Click on the 'Create You First Content Type' button.

Now select a content type of string, and then enter 'name', and click on add another field.

This time click on the 'Rich Text' type and enter 'description' and click on add another field.

Now click on the 'String' type and enter 'delivery_time' and click on add another field.

Now click on the 'Media' type and enter 'image'

Now click 'done'.

You will now have the following fields in your model:

    name -  of type String
    description - of type Rich Text
    delivery_time - of Type String
    image - of type Media

Click on 'save' at the top right of the screen.

You have now created your first Content Model.

## Add Some Takeaways

 Now lets add some example takeaways to the database. Navigate to the 'Takeaways'  Content Type which will now show in the left hand-side menu. You are now in the Content Model editor which will enable you see and edit entries.

 Click on 'Add New Takeaways' at the to right of the page.

 Add some takeaways to your system. Some samples images have been provided in the 'takeaways' folder:


```
https://github.com/martinlevington/TechOnTheTyne-HandsOnWithJS/tree/master/takeaways
```

## Configure the TakeAway API Security

Now we have some content lets set the permissions to allow out frontend application to access our content. 

When you were creating your 'Takeaway's Content Type, Strapi created for us an corresponding API, which is located at (not this will be secured by default at this point):

    http://localhost:1337/takeaways

This API will be secured by default, so we now need to enable access to it. Click on 'Roles and Permissions' in the left hand menu. We are going to set the permissions to be  Public, so now click on the 'Public' Role. 

We will set the access to the 'find' and 'findone' API endpoints. Under the Takeaway section click 'find' and 'findone' and click save.

Now visit the api endpoint located at:

http://localhost:1337/takeaways

Now do this for the Authenticated Role also.

## Querying the GraphQL Endpoint

We will now query our GraphQL endpoint. Strapi has a simple GraphQL client built in which we can use to query our API.

Go to http://localhost:1337/graphql and try the following query:

```graphql
    {
        takeaways {
            id 
            name
            image {
        	   name
                url
                mime
            } 
            delivery_time
        }
    }
```