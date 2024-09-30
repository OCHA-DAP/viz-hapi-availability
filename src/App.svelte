<script>
  import Papa from 'papaparse';
  import { onMount } from 'svelte';
  import Select from 'svelte-select';

  import Legend from './components/Legend.svelte';
  import Table from './components/Table.svelte';


  $: currentTableData = {}

  let categories = {}
  let allCategories = []
  let countries = []
  let allCountries = []
  let allTableData = {}

  const groupBy = (item) => item.group;

  const base_url = 'https://demo.hapi-humdata-org.ahconu.org/api/v1';//https://hapi.humdata.org/api/v1/';
  const app_indentifier = 'aGFwaS1kYXNoYm9hcmQ6ZXJpa2Eud2VpQHVuLm9yZw==';

  async function fetchCountryData() {
    const url = `${base_url}/metadata/location?app_identifier=${app_indentifier}&offset=0&output_format=csv&limit=10000`;

    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const csvText = await response.text();
      const parsedData = Papa.parse(csvText, {
        header: true,
        skipEmptyLines: true,
        dynamicTyping: true
      });

      let hrpCountries = [];
      parsedData.data.forEach(row => {
        if (row.has_hrp === 'True') {
          hrpCountries.push({
            value: row.code,
            label: row.name,
            //group: 'countries'
          });
        }
      });

      // save list of all countries
      allCountries = hrpCountries;

      return hrpCountries;

    } catch (error) {
      console.error('Error fetching and parsing CSV data:', error);
      return null;
    }
  }

  // async function fetchCategoryData() {
  //   const url = 'https://hapi.humdata.org/openapi.json';
    
  //   try {
  //     const response = await fetch(url);
  //     const data = await response.json();
  //     const paths = data.paths;

  //     // parse out the category and subcategory data from the paths key
  //     Object.keys(paths).forEach((path) => {
  //       const segments = path.split('/').filter(Boolean); 

  //       if (segments.length >= 3 && segments[0] === 'api' && segments[1] === 'v1') {
  //         const category = segments[2]; // cat is 3rd segment
  //         const subcategory = segments[3] || 'None'; // subcat is 4th segment

  //         // ignore "encode_app_identifier", "metadata", "util"
  //         if (['metadata', 'util', 'encode_app_identifier'].includes(category)) {
  //           return;
  //         }

  //         // build categories dict
  //         if (!categories[category]) {
  //           categories[category] = []; 
  //         }

  //         // add subcategory if not "None" and not already in array
  //         if (subcategory !== 'None' && !categories[category].includes(subcategory)) {
  //           categories[category].push(subcategory);
  //         }
  //       }
  //     });
  //   } catch (error) {
  //     console.error('Error fetching or parsing the data:', error);
  //   }
  // }

  async function getTableData() {
    let data = [];
    let offset = 0;
    let fetchedData;

    //data pagination
    do {
      let endpoint = `${base_url}/metadata/data-availability?output_format=csv&app_identifier=${app_indentifier}&offset=${offset}&limit=10000`
      fetchedData = await fetchTableData(endpoint);
      data = data.concat(fetchedData);
      offset += 10000;
    } while (fetchedData.length >= 10000);

    return data;
  }

  async function fetchTableData(endpoint) {
    try {
      const response = await fetch(endpoint);
      const text = await response.text();
      return new Promise((resolve, reject) => {
        Papa.parse(text, {
          header: true,
          complete: results => resolve(results.data),
          error: err => reject(err)
        });
      });
    } catch (error) {
      if (error.name === 'AbortError') {
        console.log('Fetch aborted');
      } else {
        throw error;
      }
    }
  }

  function getCountries(data) {
    // get unique country codes
    const uniqueCountryCodes = new Set();

    data.forEach(row => {
      const location_code = row.location_code;
      const location_name = row.location_name;

      // add country if country hasnt already been added
      if (!uniqueCountryCodes.has(location_code) && location_code !== undefined) {
        uniqueCountryCodes.add(location_code);
        // format object for select component
        countries.push({
          value: location_code,
          label: location_name,
          group: 'countries'
        });
      }
    });
    
    allCountries = countries;

    //countries.unshift({value: 'HRP', label: 'Priority Humanitarian Locations', group: 'regions'});
    return countries;
  }

  function getCategories(data) {
    data.forEach(row => {
      if (row.category !== '') {
        const category = row.category;
        const subcategory = row.subcategory;

        // if category doesn't exist, create a new array
        if (!categories[category]) {
          categories[category] = [];
        }

        // add subcategory if hasnt already been added
        if (!categories[category].includes(subcategory)) {
          categories[category].push(subcategory);
        }
      }
    });

    return categories;
  }

  function getAdminLevels(data) {
    const adminLevels = {};
    const allSubcategories = new Set();

    // get unique subcategories
    data.forEach(row => {
      allSubcategories.add(row.subcategory);
    });

    data.forEach(row => {
      if (row.location_name !== undefined) {        
        const { location_name, subcategory, admin1_name, admin2_name } = row;

        // create country and subcategory if not already created
        if (!adminLevels[location_name]) {
          adminLevels[location_name] = {};

          // make sure every country has all subcategories and set all to false to start
          allSubcategories.forEach(subcat => {
            if (subcat !== undefined) {
              adminLevels[location_name][subcat] = {
                admin0: false,
                admin1: false,
                admin2: false
              };
            }
          });
        }

        // logic to determine admin level
        if ((admin1_name === 'UNSPECIFIED' || admin1_name === '') && (admin2_name === 'UNSPECIFIED' || admin2_name === '')) {
          adminLevels[location_name][subcategory].admin0 = true;
        } 
        else if ((admin1_name !== 'UNSPECIFIED' || admin1_name === '') && (admin2_name === 'UNSPECIFIED' || admin2_name === '')) {
          adminLevels[location_name][subcategory].admin1 = true;
        } 
        else if ((admin1_name !== 'UNSPECIFIED' || admin1_name === '') && (admin2_name !== 'UNSPECIFIED' || admin2_name === '')) {
          adminLevels[location_name][subcategory].admin2 = true;
        }
      }
    });

    // sort location_names alphabetically
    const sortedAdminLevels = Object.keys(adminLevels)
      .sort()  
      .reduce((sortedObj, location_name) => {
          sortedObj[location_name] = adminLevels[location_name];
          return sortedObj;
      }, {});

    return sortedAdminLevels;
  }

  function onCountrySelect(e) {
    currentTableData = {};
    e.detail.forEach(country => {
      currentTableData[country.label] = allTableData[country.label];
    });
  }

  function onCountryClear(e) {
    allCountries.forEach(country => {
      currentTableData[country.label] = allTableData[country.label];
    });
    const sortedData = Object.entries(currentTableData)
      .sort((a, b) => a[0].localeCompare(b[0]));

    currentTableData = Object.fromEntries(sortedData);
  }

  function onCategorySelect(e) {
    console.log(currentTableData)
  }

  function onCategoryClear(e) {
  }

  function formatStr(str) {
    return str
      .split('-')
      .map(word => word.charAt(0).toUpperCase() + word.slice(1))
      .join(' ');
  }

  onMount(async () => {
    // get table data
    let data = await getTableData();
    console.log(data)

    //countries = await fetchCountryData();

    countries = getCountries(data);
    countries.sort((a, b) => a.label.localeCompare(b.label));
    
    categories = getCategories(data);
    Object.entries(categories).forEach(category => {
      allCategories.push({value: category[0], label: formatStr(category[0])})
    })
    //console.log(categories)


    allTableData = getAdminLevels(data);
    countries.forEach(country => {
      currentTableData[country.label] = allTableData[country.label];
    });

    //initTracking()
  });
</script>


<main>  
  <div class='header'>
    <h1>HAPI Location and Indicator Coverage</h1>
    <p>This table displays the available data by administrative level for each indicator and country included in HAPI.</p>
  </div>

  <div class='subheader'>
    {#if countries.length>0}
      <div class='select-wrapper'>
        <label>Filter by:</label>
        <div class='select-group'>
          <Select 
            items={allCountries} 
            clearable
            multiple
            multiFullItemClearable
            placeholder='Country'
            showChevron
            on:change={onCountrySelect}
            on:clear={onCountryClear}
          /><!--value="Priority Humanitarian Locations" {groupBy}-->
<!--           <Select 
            items={allCategories}
            clearable
            multiple
            multiFullItemClearable
            placeholder='Category'
            showChevron
            on:change={onCategorySelect}
            on:clear={onCategoryClear}
          /> -->
        </div>
      </div>
    {/if}

    {#if Object.keys(currentTableData).length > 0}
      <Legend />
    {/if}
  </div>

  {#if Object.keys(currentTableData).length > 0}
    <Table {categories} {currentTableData} />
  {/if}

</main>

<style>
  .header {
    margin-bottom: 30px;
    text-align: center;
  }
  .subheader {
    display: flex;
    justify-content: space-between;
  }
  .select-wrapper {
    margin-bottom: 30px;
    min-width: 375px;
    max-width: 900px;
    z-index: 5;
  }
  .select-group {
    display: flex;
    --border: 1px solid #CCC;    
    --border-focused: 1px solid #007CE0;
    --border-hover: 1px solid #007CE0;
    --margin: 0 10px 0 0;
    --item-hover-bg: #CCE5F9;
    --list-border: 1px solid #EEE;
  }
  .select-wrapper > div .svelte-select {
    margin-right: 10px;
  }
  .select-wrapper label {
    font-size: 14px;
    display: block;
    margin-bottom: 5px;
  }
</style>