<script>
  import Papa from 'papaparse';
  import { onMount } from 'svelte';
  import MultiSelect from 'svelte-multiselect'

  import Table from './components/Table.svelte';


  const categories = {
    'affected-people': ['refugees', 'humanitarian-needs'],
    'coordination-context': ['operational-presence', 'funding', 'conflict-event', 'national-risk'],
    'food': ['food-security', 'food-price'],
    'population-social': ['population', 'poverty-rate']
  }
  // refugees uses asylum_location_code
  // national-risk returns same data for all admin level
  // food-security returns data for some admin levels but admin codes and names are null
  // poverty-rate returns same data for all admin level

  let selected = []
  let countries = []
  let tableDict = {}

  const base_url = 'https://hapi.humdata.org/api/v1/';
  const app_indentifier = 'aGFwaS1kYXNoYm9hcmQ6ZXJpa2Eud2VpQHVuLm9yZw==';

  async function fetchData(endpoint) {
    try {
      // Fetch the CSV data from the API
      const response = await fetch(endpoint);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      // Read the response text (CSV format)
      const csvText = await response.text();

      // Parse the CSV data using PapaParse
      const parsedData = Papa.parse(csvText, {
        header: true,  // Parses the first row as field names
        skipEmptyLines: true,  // Skips empty lines
        dynamicTyping: true  // Converts numeric fields to numbers
      });

      // Return the parsed data
      return parsedData.data;

    } catch (error) {
      console.error('Error fetching and parsing CSV data:', error);
      return null;
    }
  }

  async function fetchCategoryData(categories, countries) {
    const adminLevels = [0, 1, 2]; // Admin levels to query for

    // Initialize the result dictionary
    const resultDict = {};

    // Loop through each country
    for (const country of countries) {
      // Create an empty object for the country
      resultDict[country] = {};

      // Loop through each category and subcategory
      for (const [category, subcategories] of Object.entries(categories)) {
          for (const subcategory of subcategories) {
            // Create an array to hold the results for admin levels
            const adminResults = [];

            // Loop through each admin level (0, 1, 2)
            for (const adminLevel of adminLevels) {
              const url = `${base_url}/${category}/${subcategory}?location_code=${country}&admin_level=${adminLevel}&app_identifier=${app_indentifier}`;

              try {
                // Fetch the data for the given category, subcategory, country, and admin level
                const response = await fetch(url);

                if (!response.ok) {
                  throw new Error(`Failed to fetch data for ${category}/${subcategory} in ${country} at admin level ${adminLevel}. Status: ${response.status}`);
                }

                const result = await response.json();
                const hasData = (result.data.length>0) ? true : false

                // Push true if has data, otherwise false
                adminResults.push(hasData);
              } catch (error) {
                console.error(`Error fetching data from ${url}:`, error);
                // Push false if there is an error
                adminResults.push(false);
              }
            }

            // Capitalize the subcategory name for consistency with the example output
            const formattedSubcategory = subcategory
              .split('-')
              .map(word => word.charAt(0).toUpperCase() + word.slice(1))
              .join(' ');

            // Add the results for the subcategory to the country's dictionary
            resultDict[country][formattedSubcategory] = adminResults;
          }
      }
    }

    // Log or return the final result dictionary
    console.log(resultDict);
    return resultDict;
  }


  onMount(async () => {
    const dataURL = `${base_url}/metadata/location?app_identifier=${app_indentifier}&offset=0&output_format=csv&limit=10000`;
    let countryData = await fetchData(dataURL);
    countries = countryData.map(country => country.name)

    const countryCodes = countryData.map(country => country.code)
    tableDict = fetchCategoryData(categories, countryCodes);
    console.log(tableDict)

    //initTracking()
  });
</script>


<main>  
  <div class='header'>
    <h1>HAPI Location and Indicator Coverage</h1>
    <p>This table displays the available data by administrative level for each indicator and country included in HAPI.</p>
  </div>

  {#if countries.length>0}
    <div class='select-wrapper'><MultiSelect bind:selected options={countries} /></div>
  {/if}

  <Table />

</main>

<style>
  .header {
    text-align: center;
  }
  .select-wrapper {
    margin-bottom: 30px;
    width: 350px;
  }
</style>