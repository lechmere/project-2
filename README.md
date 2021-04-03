# Weather Forecast API - GA Project Two
My second project for the General Assembly Software Engineering Immersive course, a React app pair-coded with GA classmate Rebecca Acioadea in a 48-hour hackathon.

The app is available [here](https://lechmere.github.io/Project-2/).

## Brief: Build a React application that consumes a public API
## Timeframe:
48 hours.

## Getting started
1. Access the source code via the 'Clone or download' button.
2. In CLI, run npm on the root level to install dependencies.
3. Run npm start to run program in your local environment.

## Technologies used:
- HTML5
- Bulma CSS Framework
- SCSS
- Javascript (ES6)
- React.js and JSX
- Node Package Manager (npm)
- Babel Transpiler
- Webpack
- Insomnia
- OpenWeatherMapAPI
- Axios
- react-router-dom

## Demonstration of the App Flow
<img width="1440" alt="Project2-Screenshot" src="https://user-images.githubusercontent.com/71281526/113062447-ac3c4c00-91ab-11eb-8900-5ce162a26de0.png">

The app allows for users to find the weather forecast in any city around the world. Forecasts can be provided by either utilising the search bar or the nine buttons which represent nine popular cities. After a city has been entered cards appear displaying the weather forecast. The cards have been styled so they show a light blue background when the weather is on the colder side and an orange background when the weather is warmer. The cards also provide the current temperature, the max and min temperature, and the type of weather eg. broken clouds. 

## Process
We began our project by siphoning through free APIs on the internet. Eventually we settled on the OpenWeatherMapAPI that lists the datasets of the weather forecast of over 200,000 different cities. Next, we inspected the API by hooking into it with Insomnia REST client. After, the inspection we decided to build an App that could search through the cities within this data and provide the relevant forecasts. 

- First, we created the app's wireframes.
- Secondly, we planned and wrote the pseudo code.
- Thirdly, we used VSCode Liveshare to pair program basic components in React (Home, Navbar, Forecast page) and the BrowserRouter using react-router-dom. 
- Finally, we seperated to individually code the search logic - this took longer than anticipated as we had to switch between Apis to find one that allowed us to filter through cities. 

To style our page, we combined Bulma CSS Framework and CSS. This approach was used so that effective styles could be created in a manageable time frame. 


## Search Logic
<img width="1440" alt="search-bar" src="https://user-images.githubusercontent.com/71281526/113476125-60bac400-9471-11eb-9a20-e6e248964059.png">

The search bar logic uses useStates to filter the city through the search mechanism. This occurs by using the filter function within the api, filtering to find the filteredCity which is the given city when the value entered in to the search (the cityFilter) is included in the Api's city name (search.main.name). The value of the search bar is tracked using a useState that starts as cityFilter but updates with every keystroke to updateCityFilter. When the search button is clicked, the page refreshes to the cityFilter's page to present the results.

    // SEARCH BAR FILTER CITY LOGIC ---------------------------------------------
```
const Search = () => {
  const [api, updateApi] = useState([])
  const [cityFilter, updateCityFilter] = useState('')

  useEffect(() => {
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=London&appid=8569449663f37c3e52851a11d8ac7e5c`)
      .then(resp => {
        updateApi(resp.data)
        console.log(resp.data)
      })
  }, [])

console.log(api)
  function filterCity() {
    const filteredCity = api.filter(search => {
      const name = search.main.name.toLowerCase()
      const filterText = cityFilter.toLowerCase()
      return name.includes(filterText)
    })
    return filteredCity
  }
```
<img width="1440" alt="search-result" src="https://user-images.githubusercontent.com/71281526/113476132-66b0a500-9471-11eb-8f0e-b4e298044cf1.png">

The SearchResult requirs the use of props, params and useState to work. The searchId from the props, is entered in to the Api url of the axios get so the right city database is received, presenting the forecast. The search result page also creates a loading page if the search is not found.

      // SEARCH RESULT & LOADING LOGIC ---------------------------------------------
```
const SearchResult = (props) => {
  const searchId = props.match.params.searchId

  const [search, updateSearch] = useState({})

  useEffect(() => {
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${searchId}&appid=8569449663f37c3e52851a11d8ac7e5c`)
      .then(resp => {
        const data = resp.data
        updateSearch(data)
        console.log(data)
      })
  }, [searchId])
console.log(search)

  if (!search.name) {
    return <div className="background">
      <h2 className="loading"> Loading... </h2>
    </div>
  }
 ```
The search results page has unique stlying of the cards. For this we used inline styling. First, we had to convert the temperatures from Kelvin to Celsius by minusing 273.15 and then we coded logic to alter the background colour and the text colour of the card. These colour changes are dependant on the temperature being above or below specific numbers (18, 15, 30).
 
      // SEARCH RESULT STYLE LOGIC ---------------------------------------------
```
return  <div className="background">
        ...
        <div className="card" style={{ backgroundColor: (((search.main.temp) - 273.15) > 18 ? 'rgb(253, 214, 84)' : 'lightBlue'), border: '10px solid lightgrey' }}  >
        ...
                <p className="subtitle is-10" style={{ color: (((search.main.temp) - 273.15) > 18 ? 'orange' : 'darkBlue') }}> {Math.round((search.main.temp) - 273.15)}°C</p>
        ...
                  <p className="card-footer-item" style={{ color: (((search.main.temp) - 273.15) < 15 ? 'mediumBlue' : 'black') }}>
                      LOW: {Math.round((search.main.temp_min) - 273.15)}°C
                  </p>
                  <p className="card-footer-item" style={{ color: (((search.main.temp) - 273.15) > 30 ? 'red' : 'black') }}>
                      HIGH: {Math.round((search.main.temp_max) - 273.15)}°C
                  </p>
        ...
```

## Challenges
Building my first React app with a partner proved to have a few technical difficulties. VSCode Liveshare was incredibly flawed, and made it hard to comfortably code alone. We overcame these challenges by editing code within our own files and then pasting this code back in our shared version when necessary.

## Wins
I found pair-coding very beneficial. My partner and I communicated well and respected eachothers opinions and ideas. This meant that we were always helping eachother work towards our individual and overall project goals.

## Future features
In the future, it would be exciting to add to the design features of this project. This could be executed through use of animation or video. Another possible feature, could be encorporating a fact Api that offers trivia on the given city.

## Key learnings
My knowledge of using Insomnia to inspect and manipulate Apis was greatly reinforced by this project. In addition to my Insomnia skills, my technical communication benefited hugely. This is because our pair coding took place remotely, meaning all challenges had to be overcome whilst using very limited visuals and pure conversation.

# Weather Forecast API - GA Project Two
My second project for the General Assembly Software Engineering Immersive course, a React app pair-coded with GA classmate Rebecca Acioadea in a 48-hour hackathon.



