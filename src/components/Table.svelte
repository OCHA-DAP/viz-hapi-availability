<script>
  import { onMount } from 'svelte';

  let table, tableCells, tooltip;

  const hoverColor = '#FEF1EF';


  function initEvents() {
    // get table elements
    table = document.getElementById('coverageTable');
    tableCells = document.querySelectorAll('#coverageTable td, #coverageTable th');

    // assign mouse events
    tableCells.forEach(cell => {
      cell.addEventListener('mouseover', function() {
        if (!cell.classList.contains('fixed-col') && cell.tagName !== 'TH') {
          highlightCells(cell);

          // get position and dimensions of hovered cell
          // const rect = cell.getBoundingClientRect();
          // const cellHeight = rect.height;


        }
      });

      cell.addEventListener('mouseout', function() {
        resetCells();
      });
    });
  }

  // highlight hovered cell and cells to left and above
  function highlightCells(cell) {
    // get current cell position
    const row = cell.parentElement;
    const rowIndex = Array.from(table.rows).indexOf(row);
    const cellIndex = Array.from(row.cells).indexOf(cell);

    // highlight cells above current cell in same column
    for (let i = 0; i <= rowIndex; i++) {
      table.rows[i].cells[cellIndex].style.backgroundColor = hoverColor;
    }

    // highlight cells to the left in same row
    for (let i=0; i <=cellIndex; i++) {
      row.cells[i].style.backgroundColor = hoverColor;
    }
  }

  function resetCells() {
    tableCells.forEach(cell => {
      cell.style.backgroundColor = '';
    });
  }

  onMount(async () => {
    tooltip = document.querySelector('.tooltip')
    initEvents();
  });
</script>

<div class='container'>
  <div class='table-wrapper'>
    <table id='coverageTable'>
      <thead>
        <tr>
          <th class='fixed-col'></th>
          <th>Humanitarian Needs<br><span><a href='https://hapi.humdata.org/docs#/Affected%20people/get_humanitarian_needs_api_v1_affected_people_humanitarian_needs_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Refugees<br><span><a href='https://hapi.humdata.org/docs#/Affected%20people/get_refugees_api_v1_affected_people_refugees_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Conflict Events<br><span><a href='https://hapi.humdata.org/docs#/Coordination%20%26%20Context/get_conflict_events_api_v1_coordination_context_conflict_event_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Funding<br><span><a href='https://hapi.humdata.org/docs#/Coordination%20%26%20Context/get_fundings_api_v1_coordination_context_funding_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>National Risk<br><span><a href='https://hapi.humdata.org/docs#/Coordination%20%26%20Context/get_national_risks_api_v1_coordination_context_national_risk_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Operational Presence<br><span><a href='https://hapi.humdata.org/docs#/Coordination%20%26%20Context/get_operational_presences_api_v1_coordination_context_operational_presence_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Food Prices<br><span><a href='https://hapi.humdata.org/docs#/Food%20Security%20%26%20Nutrition/get_food_prices_api_v1_food_food_price_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Food Security<br><span><a href='https://hapi.humdata.org/docs#/Food%20Security%20%26%20Nutrition/get_food_security_api_v1_food_food_security_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Baseline Population<br><span><a href='https://hapi.humdata.org/docs#/Population%20%26%20Socio-Economy/get_populations_api_v1_population_social_population_get' target='_blank'>Go to API Sandbox</a></span></th>
          <th>Poverty Rate<br><span><a href='https://hapi.humdata.org/docs#/Population%20%26%20Socio-Economy/get_poverty_rates_api_v1_population_social_poverty_rate_get' target='_blank'>Go to API Sandbox</a></span></th>
        </tr>
      </thead>
      <tbody>
          <tr>
            <td class='fixed-col'><div class='country'>Afghanistan</div></td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Burkina Faso</div></td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Cameroon</div></td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon hide'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
            <td>
              <div class='admin-key'>
                <div class='admin-icon'>0</div>
                <div class='admin-icon hide'>1</div> 
                <div class='admin-icon hide'>2</div>
              </div>
            </td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Central African Republic</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Chad</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Colombia</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Democratic Republic of the Congo</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>El Salvador</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Ethiopia</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Guatemala</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Haiti</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Hondura</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Mali</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Mozambique</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Myanmar</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Niger</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Nigeria</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>State of Palestine</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Somalia</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>South Sudan</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Sudan</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Syrian Arab Republic</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Ukraine</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Venezuela (Bolivarian Republic of)</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td class='fixed-col'><div class='country'>Yemen</div></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
      </tbody>
    </table>
  </div>

  <div class='gradient-overlay'></div>
  <div class='tooltip'></div>
</div>


<style lang='scss'>
  .container {
    position: relative;
  }
  .table-wrapper {
    display: block;
    height: 600px;
    margin: 0;
    overflow: auto;
    padding: 0;
    position: relative;
    width: 100%;
  }
  table {
    border: 0;
    border-collapse: separate;
    border-spacing: 0;
    margin-right: 50px;
    width: 100%;
  }

  th, td {
    border-right: 1px solid #E6E9F5;
    line-height: 16px;
    padding: 8px 12px;
  }

  td {
    border-top: 1px solid #E6E9F5;
  }

  thead th {
    background-color: #FFF;
    font-family: 'Gotham-Bold';
    font-size: 14px;
    line-height: 16px;
    position: sticky;
    padding: 10px 12px;
    top: 0;
  }

  th span {
    display: block;
    font-family: 'Source Sans 3', sans-serif;
    font-size: 14px;
    font-weight: 400;
    padding: 4px 0;
  }

  .fixed-col {
    background-color: #FFF;
    font-family: 'Gotham-Bold';
    font-size: 14px;
    line-height: 18px;
    left: 0;
    position: -webkit-sticky;
    position: sticky;
    text-transform: uppercase;
    width: 300px;
    z-index: 3;
  }
  .fixed-col .country {
    padding: 10px 12px;
    width: 300px;
  }

  thead .fixed-col {
    border-top: 0;
  }

  tbody .fixed-col {
    z-index: 1;
  }

  .admin-key {
    display: flex;
    flex-flow: row; 
    justify-content: center;
    width: 166px;
  }
  .admin-icon {
    align-items: center;
    background-color: #1EBFB3;
    border-radius: 50%;
    color: #FFF; 
    display: flex;
    font-family: 'Gotham-Bold';
    font-size: 14px; 
    height: 20px;
    justify-content: center;
    margin: 0 2px;
    width: 20px; 
    &.hide {
     opacity: 0;
    }
  }

  .gradient-overlay {
    background: linear-gradient(to right, rgba(255, 255, 255, 0) 0%, rgba(255, 255, 255, 1) 100%);
    bottom: 0;
    right: 0;
    top: 0;
    pointer-events: none;
    position: absolute;
    width: 100px;
  }
</style>