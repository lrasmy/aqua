# Application name:

<p align="center">
  <img src="https://github.com/Niloofar-Sh/aqua/blob/main/src/assets/images/logo_aqua-1.jpg" alt="interface" width="500" height="300"> 
  <br/> 
  </img>
</p>

# Contents:
* [Analysis of the existing SPARC search portal](#chart_with_upwards_trend-analysis-of-the-existing-sparc-search-portal)
* [AQUA objectives for the new SPARC search portal](#bulb-aqua-objectives-for-the-new-sparc-search-portal)
* [AQUA Components](#iphone-aqua-components)
  * [AQUA Backend](#1-aqua-backend)
  * [AQUA UI](#2-aqua-ui)
* [How to use AQUA?](#information_desk_person-how-to-use-aqua)
  * [Installation](##hammer_and_wrench-installation)
  * [Dependencies](##electric_plug-dependencies)
* [Testing](#mag_right-testing)
* [Examples](#round_pushpin-examples)
* [Ideas?](#speech_balloon-ideas)


# :chart_with_upwards_trend: Analysis of the existing SPARC search portal

**1. Limited search feature of the SPARC Portal:** It does not recognize nearby words (in case of **typos**). As an example, if we type "rattis" (typo) instead of "rattus", it does not recognize it or give any suggestion (Fig 1).
<br/>
<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/blob/main/src/assets/images/rattis_current_result.jpg" width="800" height="450"></br>
  <i>Fig 1. Search results for a typo (rattis) on the SPARC portal. </i>
</p>
  
   <br/>
   
**2. Vague result display:** You need to enter the exact correct keywords in the search bar (Fig 2) and yet, it does not **bold/highlight the search keywords** among the search results.

<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/blob/main/src/assets/images/rattus_current_result.jpg" width="800" height="450"></br>
  <i>Fig 2. Search results for a correct keyword (rattis) on the SPARC portal. Keywords are not bolded/highlighted in the results.</i>
</p>
   
**3. Limited result filterings:** The website currently refines the results by either being "Public" or "Embargoed" (Fig 3).

**4. Limited result sorting:** The website currently sorts the results by either "Title" (listed alphabetically) or "Published date" (Fig 3).

<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/blob/main/src/assets/images/rattus_filters.jpg" width="800" height="450"></br>
  <i>Fig 3. Limited filterings for the results display on the SPARC portal.</i>
</p>
   <br/>
   
**5. No option for Email alerts :** In the event of No results being found for the queries, there is no way for the user to get informed on whether a **new dataset/resource** (related to their query) has been published on the SPARC portal or not.
<br/>

# :bulb: AQUA objectives for the new SPARC search portal
Specific features of AQUA are listed below:
<br/>
* :point_right: __Query refinement__
   * Auto-completion:<br/>
      Based on the term, our tool automatically completes the queries if it partially/completely matches any keywords. It then sends the selected keyword to AQUA backend.
   * Suggestion:<br/>
      If no exact matches are found, it finds close-matches and suggests them to the user with popping up the phrase: *"Did you mean ...?"*. Otherwise, it will send the raw and uncorrected query to AQUA backend.

* :point_right: __Results filtering__
   * Sort by:<br/>
      When the results for the query are displayed, user will have the option of sorting them based on the *Relevance*, *Date published*, and *Alphabetical order*.
    * Filter by:<br/>
      The results can also be filtered based on *Keyword*, *Author*, *Category*, and *Publication date*.
    * Matched text bolded/highlighted
    
    
* :point_right: __Alert me about future datasets related to search query__

     At the end, if no results are returned by the AQUA backend, our tool asks the user if they want to get notified when a related resource is published or not. For a given email address, the tool checks for its validity and then stores it using SQLite. Thereafter, it will check for any updated/uploaded related resource on the SPARC portal everyday at 2AM EDT. In case of the requested resource availability, it sends a notification email to the registered user. 
<br/>
<p align="center">   
  <b>The AQUA pipeline:</b>
</p>
<br/>
<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/blob/main/src/assets/images/aqua_pipeline.jpg" width="550" height="1000"></br>
  <i>Fig 4. AQUA pipeline including three major sections: Query refinement, Results filtering, and Notify me.</i>
</p>
   <br/>
   


# :iphone: AQUA Components
The AQUA application integrates Python libraries, data mining tools, a SQL database engine, and Document Object Model (DOM) API to mimic an environment similar to the SPARC search portal with an improved functionality in multiple ways. In general, the AQUA platform consists of a presentation layer as the User Interface (UI) (referred to as frontend) and a server-side data-access layer (referred to as backend). The **AQUA UI** and the **AQUA Backend** bridge between the user and the Knowledge Management Core (K-Core) database. K-Core is the SPARC knowledge graph database.


## 1. AQUA Backend

The AQUA backend includes querying the K-Core database for information, delivering data to the frontend, and processing any logic that the AQUA UI requires. The main tools utilised for the AQUA Backend are Python (Jupyter Lab), Docker, SQLite, and SciGraph.
<br/>
The AQUA backend focuses on two main features:
<br/>

:sparkles: __Behind the scenes of AQUA's Query refinement__ 

<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/raw/main/src/assets/images/Suggestion%26AutoComplete.jpg" alt="interface" width="780" height="500"></br>
  <i>Fig 5. Query refinement by Auto-completion/Suggestions.</i>
</p>
<br/>
   AQUA utilises SciGraph for auto-completion and suggestion. However, we found that SciGraph’s suggestions do not deal with query problems such as error spelling and continuous script (*scriptio continua*). Therefore, we have added a new auto-correction feature to segment queries with missing spaces and fix error spelling by creating a pipeline to [SymSpellPy](https://pypi.org/project/symspellpy/). The auto-correction result is combined with the suggestion results and then executed as the final query search terms.
(To read more visit: ["Query refinement" Readme](../blob/main/Documentation/QueryRefinement.md))  

<br/>
<br/>

:sparkles: __Behind the scenes of AQUA's Email notification__ 
<br/>
<p align="center">
   <img src="https://github.com/Niloofar-Sh/aqua/raw/main/src/assets/images/NotifyMe.jpeg" alt="interface" width="780" height="500"></br>
  <i>Fig 6. "Notify me" module.</i>
</p>
 <br/>
The "Notify Me" option is to send emails that summarize search results against exact keywords. "Notify Me" module sends the email just once at least one exact hitting match exists. Therefore, "Notify Me" can be used for:<br/>
<br/>

1.	Notify users when a dataset gets published against keywords that do not retrieve any results yet.
2.	Emailing the current search results in a tabular format, which can be found helpful for users.
Additionally, the "Notify Me" module stores all requests in an SQLite database, which the SPARC team can further analyze to understand the search pattern and get more insights on demand. For example, the SPARC team can find the most common keywords searched with no existing matches and decide to fulfil such needs. 

(To read more visit: ["Notify me" Readme](https://github.com/Niloofar-Sh/aqua/blob/main/Documentation/NotifyMe.md))
<br/>

## 2. AQUA UI
<br/>
AQUA UI receives the user's queries, formulates them, and transfers to the AQUA Backend module. When the response from the AQUA backend is received, the AQUA UI interprets it and displays the content on the screen. Like the SPARC portal web application, the AQUA UI is implemented by the HTML-CSS-JS trio using: [VueJS](https://vuejs.org/) and [NuxtJS](https://nuxtjs.org/).

# :information_desk_person: How to use AQUA? 
blah blah blah

## :hammer_and_wrench: Installation
You can setup and deploy the Docker module for AQUA by following the steps in: [AQUA Docker](https://github.com/Niloofar-Sh/aqua/tree/main/aqua_docker#readme)
## :electric_plug: Dependencies

[Docker](https://www.docker.com/) is a platform that bundles and delivers software in packages called containers.

# :mag_right: Testing

# :round_pushpin: Examples
Here, we have provided examples of how AQUA improves the search experience in two scenarios, i.e.:  <br/>
1) AQUA auto-completion feature helps the user find a correct keyword while typing (Fig x), and
2) AQUA suggests close-matches for a query with typo (Fig xx).  
The user's decision to either select one of the close-matches or go with thier raw, uncorrected query leads to "Result(s) display" or "No results" notification (Fig xxx). When at least one result is returned, AQUA displays options to sort or filter the results. This is shown with an example in Fig xxxx.

 
# :speech_balloon: Ideas?
To share your ideas, feedback, and comments contact any of our team members.

