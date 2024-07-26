<script>
  
  import { getContext } from "svelte"	
  import '@fullcalendar/core/locales-all'
  import FullCalendar from 'svelte-fullcalendar';
  import daygridPlugin from '@fullcalendar/daygrid';
  import timeGridPlugin from '@fullcalendar/timegrid';
  import listPlugin from '@fullcalendar/list';
  import { onMount } from "svelte";
  import {langs, codeLang} from "./lang"
 
  export let language
  export let calendarEvent
  
  export let mappingTitle
  export let mappingDate
  export let mappingStart
  export let mappingEnd
 
  export let mappingTitle2
  export let mappingDate2
  export let mappingStart2
  export let mappingEnd2

  export let dataProvider
  export let dataProvider2

  export let mappingColor
  export let mappingColor2

  export let allday
  export let allday2
  
  export let headerOptionsStart
  export let headerOptionsCenter
  export let headerOptionsEnd

  let eventsList = []
  let options;

  function calculateEvents(){
    
    if(eventsList.length > 0){
      eventsList = []
    }
    if(dataProvider.rows){
      dataProvider.rows.forEach(event => {
        let eventColor = mappingColor ?? '#313131'           
        eventsList.push({ title: event[mappingTitle], date: event[mappingDate], start: event[mappingStart], end: event[mappingEnd], color: eventColor, event: event, allDay: allday   })        
      });
    }
    eventsList = eventsList;
options = {
    headerToolbar: {
      start: headerOptionsStart,
      center: headerOptionsCenter,
      end: headerOptionsEnd
    },
    plugins: [daygridPlugin, listPlugin, timeGridPlugin],
    initialDate:  Date.now(),
    locale: language,
    dayMaxEvents: true,
    eventClick: (event)=>{
      calendarEvent({
        value: event.event
      })
      console.log(JSON.parse(text))
      console.log(event.event.title)
    },
    events:eventsList,
    eventColor: '#378006',
    theme: true,
    ...langs[codeLang(language)]
  }
  }
  $: dataProvider.rows, calculateEvents();

  const { styleable } = getContext("sdk") 
  const component = getContext("component")

</script>

<div use:styleable={$component.styles}>
  {#if options}
  <FullCalendar {options} />
  {/if}
</div>
