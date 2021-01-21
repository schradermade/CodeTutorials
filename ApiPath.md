<p align="center">
  <u><big><b>Pathway of the API Request</b></big></u>
  <br><br>
  <u><b>by Nathan Schrader - January 8th, 2021</b></u>
</p>

### 1. `CretaceousClient > Views > Animals > Index.cshtml` <br>

### 2. `Click on "View Details" of a particular animal in the browser` <br>

<b>Example Links:</b>

--------------------[Name of Link]-------[Route]-----[Controller]-----[Pass in animal ID] <br>
`@Html.ActionLink(  "View Details"  ,   "Details" ,   "Animals" ,    new {id = Model[0].AnimalId})` <br>

------------[Controller]----[Route]----[Pass in AnimalId] <br>
`<a href="/      Animals     ,   Details     ,     @(animal.AnimalId)  "><p>View Details</p></a>` <br>

### 3. `AnimalsController.cs > "Details" GET Route` <br>
  <ol>
    <li>Passes in the animal id as parameter (from "View Details" link we just clicked)</li>
    <li>Going into Animal.cs, going to .GetDetails method and passing in ID</li>
  </ol>

### 4. `Animal.cs > GetDetails method` <br>
  <ol>
    <li>Go to GetDetails method</li>
      <ol>
        <li>Pass in the AnimalId property</li>
        <li>Go to ApiHelper class > .Get method, and pass in AnimalId property</li>
      </ol>
  </ol>

### 5. `ApiHelper > "Get" method (not to be confused with a GET request)` <br>
  <ol>
    <li>Get method passing in AnimalId property</li>
    <li>RestClient: Make an API request to address "localhost:5000" (or from wherever)</li>
    <li>RestRequest: Make a .GET request for "animals/{id} - This is the "endpoint"?</li>
    <li>Creating a response "container" with client.ExecuteTaskAsync(request) and dropping in the information received from "request"</li>
    <li>Return the "Content" of the response: response.Content</li>
  </ol>

<p align="center">
<br><br>
  <u><big><b>From this point, we reverse our path</b></big></u>
  <br><br>
</p>

### 6. `Back to Animal.cs, to GetDetails method` <br>
<ol>
  <li>apiCallTask = "response.Content" which was the return from the Get method</li>
      in ApiHelper in Step 5
  <li>Turning it into Json Object, DeserializeObject by passing in "result"</li>
  <li>Return this infomration "animal" to AnimalsController >Details Route</li>
</ol>

### 7. `AnimalsController > Details Route` <br>
  <ol>
    <li>Passing in the info recieved from Step 6 - "animal" - and assigning it
     "animalDetails"</li>
    <li>Return "animalDetails" to the View (the animal Details View)</li>
  </ol>

### 8. `Views > Animals > Details.cshtml` <br>
  <ol>
    <li>Now we pass in this information to the View by:</li>
      <ul>
        <li>"@Model.Name"</li>
        <li>"@Model.Species"</li>
        <li>"@Model.Age"</li>
        <li>"@Model.Gender"</li>
      </ul>
  </ol>

<hr />

<strong>Note: Whatever is passed into the "return View" of the Route Handler
        will become the "Model" in the view. Therefore @Model.Name is the "Name"
        property of the particular animal that we just made an API call on.</strong>