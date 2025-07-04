{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "config": {
    "locale": {
      "time": {
        "dateTime": "%A, %e de %B de %Y, %X",
        "date": "%d/%m/%Y",
        "time": "%H:%M:%S",
        "periods": ["AM", "PM"],
        "days": ["domingo", "lunes", "martes", "miércoles", "jueves", "viernes", "sábado"],
        "shortDays": ["dom", "lun", "mar", "mié", "jue", "vie", "sáb"],
        "months": [
          "enero", "febrero", "marzo", "abril", "mayo", "junio",
          "julio", "agosto", "septiembre", "octubre", "noviembre", "diciembre"
        ],
        "shortMonths": [
          "ene", "feb", "mar", "abr", "may", "jun",
          "jul", "ago", "sep", "oct", "nov", "dic"
        ]
      }
    }
  },
  "background": "transparent",
  "usermeta": {
    "version": "01.01",
    "developedBy": "Madison Giammaria",
    "gitHub": "https://github.com/Giammaria",
    "linkedIn": "https://www.linkedin.com/in/madison-giammaria-58463b33",
    "email": "giammariam@gmail.com",
    "visualName": "Hierarchical Gantt",
    "visualDescription": "A hierarchical Gantt visual heavily inspired by Davide Bacci's Gantt (https://github.com/PBI-David/Deneb-Showcase?tab=readme-ov-file#gantt-chart). This Gantt takes a hierarchical (tree) data structure (https://medium.com/@pixelprofits/data-structures-deep-dive-4-8-trees-hierarchical-data-representation-f7deba97e294).",
    "interactivity": [
      {"event": "mousewheel", "interaction": "vertical scroll"},
      {"event": "shiftKey + mousewheel", "interaction": "horizontal scroll"},
      {
        "event": "ctrlKey + mousewheel",
        "interaction": "change date granularity"
      }
    ]
  },
  "background": "transparent",
  "usermeta": {
    "version": "01.01",
    "developedBy": "Madison Giammaria",
    "gitHub": "https://github.com/Giammaria",
    "linkedIn": "https://www.linkedin.com/in/madison-giammaria-58463b33",
    "email": "giammariam@gmail.com",
    "visualName": "Hierarchical Gantt",
    "visualDescription": "A hierarchical Gantt visual heavily inspired by Davide Bacci's Gantt (https://github.com/PBI-David/Deneb-Showcase?tab=readme-ov-file#gantt-chart). This Gantt takes a hierarchical (tree) data structure (https://medium.com/@pixelprofits/data-structures-deep-dive-4-8-trees-hierarchical-data-representation-f7deba97e294).",
    "interactivity": [
      {"event": "mousewheel", "interaction": "vertical scroll"},
      {"event": "shiftKey + mousewheel", "interaction": "horizontal scroll"},
      {
        "event": "ctrlKey + mousewheel",
        "interaction": "change date granularity"
      }
    ]
  },
  "height": 580,
  "signals": [
    {
      "name": "configDesiredTotalVisualWidth",
      "description": "The approximate width of the visual. The actual width is impacted by whether vertical scroll is needed.",
      "update": "1400"
    },
    {
      "name": "configDateStepSizePercentInital",
      "description": "The initial date granularity % (value between 0 and 1). Once your data is loaded, you'll want to adjust the date granularity slider to your desired view, then look at the 'dateStepSizePercent' signal in the Signal Viewer pane. Use that value as the initial value here.",
      "value": 0.257
    },
    {
      "name": "configIncludeRoot",
      "description": "boolean to indicate whether the root node should be visible",
      "init": "false"
    },
    {
      "name": "configInitialLevel",
      "description": "the number of levels deep to start with",
      "value": 1
    },
    {
      "name": "configIncludedLeftHandColumns",
      "description": "array of column titles to be included. 'name' must be included. 'startDate', 'endDate', 'duration', and 'progress' are all 'optional'",
      "value": ["name", "startDate", "endDate", "duration", "progress"]
    },
    {
      "name": "configRow",
      "description": "configurations for the rows",
      "init": "{rowHeight: 50, levelIndentWidth: 15, defaultFill: '#40407d'}"
    },
    {
      "name": "configColumn",
      "description": "configurations for the columns that appear to the left of the Gantt",
      "init": "{xOffset: 5, innerPadding: 25, columns: {'taskTitle': {allowableWidth: 450, font: 'Segoe UI', fontSize: 16, align: 'left', fontWeight: 600}, 'startDate': {allowableWidth: 65, font: 'Segoe UI', fontSize: 16, align: 'right'}, 'endDate': {allowableWidth: 65, font: 'Segoe UI', fontSize: 16, align: 'right'}, 'duration': {allowableWidth: 55, font: 'Segoe UI', fontSize: 16, 'align': 'right'}, 'progress': {allowableWidth: 56, font: 'Segoe UI', fontSize: 16, cornerRadius: 4.5}}}"
    },
    {
      "name": "configGantt",
      "description": "configurations for the Gantt",
      "update": "{x: ganttRange[0], childRect: {cornerRadius: 1, percentOfRowHeight: 0.5}, parentRect: {percentOfRowHeight: 0.25}, label: {xOffset: 7.5, font: 'Segoe UI', fontSize: 12, align: 'left', fontWeight: 400, fill: '#999'}, todayLine: {label: {text: 'Today'}, stroke: '#d6022d', strokeWidth: 0.5, strokeDash: [2,3] }}"
    },
    {
      "name": "configVerticalScrollbar",
      "description": "configurations for the vertical scroll bar",
      "update": "{enabled: actualHeight>adjustedHeight,innerPadding: 10, track: {width: 10, height: extent([actualHeight, adjustedHeight])[0], fill: '#F3F3F3'}, handle: {height: max((adjustedHeight/actualHeight)*extent([actualHeight, adjustedHeight])[0], 30), fill: '#ddd', hover: {fill: '#888'}}}"
    },
    {
      "name": "configHorizontalScrollbar",
      "description": "configurations for the horizontal scroll bar",
      "update": "{enabled: length(data('dateWindowSize')) > 0 ? data('dateWindowSize')[0]['scrollEnabled'] : false, innerPadding: 10, track: {height: 10, width: ganttRange[1]-ganttRange[0], fill: '#F3F3F3'}, handle: {width:  length(data('dateWindowSize')) <= 0 ? 30 : max((ganttRange[1]-ganttRange[0])*(data('dateWindowSize')[0]['windowRowCount']/data('dateWindowSize')[0]['rowCount']), 30), fill: '#ddd', hover: {fill: '#888'}}}"
    },
    {
      "name": "configHeader",
      "description": "configurations for the top area that contains the controls",
      "update": "{height: 60, yOffset: -35}"
    },
    {
      "name": "configButtons",
      "description": "configurations for the button controls",
      "init": "{padding: 5, yOffset: 5, label: {text: 'Expandir/Colapsar', font: 'Segoe UI', fontSize: 18, fill: '#666', fontStyle: 'regular', dy: 15}, outerStroke: '#777', fill: '#EEE', hoverFill: 'steelblue'}"
    },
    {
      "name": "showDetailsConfig",
      "description": "configurations for the show details toggle control",
      "update": "{enabled: true, initialValue: true, xOffset: 90, track: {height: 10, width: 25, cornerRadius: 5, fill: '#EEE', stroke: '#777', strokeWidth: 1}, handle: {stroke: '#777', strokeWidth: 1, fill: '#fff'}, label: {text: 'Detalle', font: 'Segoe UI', fontSize: 19, fill: '#666', fontStyle: 'regular', dy: 11}, on: {fill: '#2cb82c', fillOpacity: 1, stroke: '#777', strokeWidth: 1}, tooltip: {text: 'Muestra/Oculta Detalle de columnas'}}"
    },
    {
  "name": "columnsToShow",
  "description": "Lista de columnas visibles según showDetails",
  "update": "showDetails ? configIncludedLeftHandColumns : ['name']"
}
,
    {
  "name": "configDateStepRange",
  "description": "adjust the nested array to determine the range for step sizes for day, month, and year",
  "update": "[[(ganttRange[1]-ganttRange[0])/3, 20], [(ganttRange[1]-ganttRange[0])/3, 40], [(ganttRange[1]-ganttRange[0])/3, 40]][indexof(['day','month','year'], dateGranularity)]"
},
    {
      "name": "configDateStepSlider",
      "description": "configurations for the date granularity slider control",
      "update": "{enabled: true, innerPadding: 10, track: {height: 7.5, width: 130, cornerRadius: 5, fill: '#EEE', stroke: '#777', strokeWidth: 1}, handle: {outerStroke: '#777', outerStrokeWidth: 1, outerfill: '#fff', innerStroke: '#777', innerStrokeWidth: 1, innerFill: '#666'}, label: {text: 'Nivel Fecha', font: 'Segoe UI', fontSize: 14, fill: '#666', fontStyle: 'regular', dy: 10}, progress: {fill: '#2cb82c', fillOpacity: 1}, tooltip: {text: 'Ajustar'}, reset: {iconPath: 'M75 75L41 41C25.9 25.9 0 36.6 0 57.9L0 168c0 13.3 10.7 24 24 24l110.1 0c21.4 0 32.1-25.9 17-41l-30.8-30.8C155 85.5 203 64 256 64c106 0 192 86 192 192s-86 192-192 192c-40.8 0-78.6-12.7-109.7-34.4c-14.5-10.1-34.4-6.6-44.6 7.9s-6.6 34.4 7.9 44.6C151.2 495 201.7 512 256 512c141.4 0 256-114.6 256-256S397.4 0 256 0C185.3 0 121.3 28.7 75 75zm181 53c-13.3 0-24 10.7-24 24l0 104c0 6.4 2.5 12.5 7 17l72 72c9.4 9.4 24.6 9.4 33.9 0s9.4-24.6 0-33.9l-65-65 0-94.1c0-13.3-10.7-24-24-24z', tooltipText: 'Reset Granularity', fill:'#888', hoverFill: '#333'}}"
    },
    {
      "name": "configInfo",
      "description": "configurations for the info that appears when hovering over the info icon",
      "update": "{enabled: true, fill: '#979797', hoverFill: '#598fbc', size: 0.0025, x: 10, y: extent([actualHeight, adjustedHeight])[0] +configHorizontalScrollbar.innerPadding, stroke: '#777', strokeWidth: 0.5, tooltip: {'Scroll Vertical:': 'Rueda Raton', 'Scroll Horizontal :': 'shift+ Rueda', 'Date granularity:': 'ctrl+Rueda', '‎': '‎', '‎‎': '* doble click para ocultar'}, iconPath: 'M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zM216 336l24 0 0-64-24 0c-13.3 0-24-10.7-24-24s10.7-24 24-24l48 0c13.3 0 24 10.7 24 24l0 88 8 0c13.3 0 24 10.7 24 24s-10.7 24-24 24l-80 0c-13.3 0-24-10.7-24-24s10.7-24 24-24zm40-208a32 32 0 1 1 0 64 32 32 0 1 1 0-64z'}"
    },
    {
      "name": "showInfoIcon",
      "description": "boolean to indicate whether the info icon is currently visible",
      "init": "configInfo.enabled",
      "on": [
        {"events": {"type": "dblclick"}, "update": "!configInfo.enabled ? false : !showInfoIcon"}
      ]
    },
    {
      "name": "currentMaxLevel",
      "description": "the current maximum depth for all visible nodes",
      "update": "data('currentMaxIndentWidth')[0]['currentMaxLevel']"
    },
    {
      "name": "currentMaxIndentWidth",
      "description": "the current maximum indent width for all visible nodes",
      "update": "data('currentMaxIndentWidth')[0]['indent']"
    },
    {
      "name": "expandAllClicked",
      "description": "boolean indicating whether expandAll was the last element clicked",
      "init": "false",
      "on": [
        {
          "events": "@expandAll_collapseAll_buttons:click",
          "update": "!isValid(datum.datum) ? resetLevel : datum.datum.name === 'expandAll' ? true : false"
        },
        {
          "events": "@row_clickable_rect:click[!event.ctrlKey]",
          "update": "false"
        }
      ]
    },
    {
      "name": "resetLevel",
      "description": "the number of levels deep to revert to when collapsing all nodes",
      "init": "configInitialLevel",
      "on": [
        {
          "events": "@expandAll_collapseAll_buttons:click",
          "update": "!isValid(datum.datum) ? resetLevel : datum.datum.name === 'expandAll' ? 9999999999999 : configInitialLevel"
        }
      ]
    },
    {
      "name": "isInitial",
      "description": "boolean indicating if this is considered the initial expand/collapse state",
      "init": "true",
      "on": [
        {
          "events": "@row_clickable_rect:click[!event.ctrlKey]",
          "update": "isValid(datum) && datum.hasChildren ? false : isInitial"
        },
        {"events": "@expandAll_collapseAll_buttons:click", "update": "true"}
      ]
    },
    {
      "name": "expandCollapseButtonDatum",
      "description": "the datum associated with the expand/collapse button that is currently being interacted with",
      "value": null,
      "on": [
        {
          "events": "@expandAll_collapseAll_buttons:mouseover",
          "update": "datum.datum"
        },
        {"events": "@expandAll_collapseAll_buttons:mouseout", "update": "null"}
      ]
    },
    {
      "name": "lastClickedNode",
      "description": "node in the hierarchy with children that was last expanded or collapsed",
      "init": "null",
      "on": [
        {
          "events": "@row_clickable_rect:click[!event.ctrlKey]",
          "update": "isValid(datum) && datum.hasChildren ? {timestamp: now(), datum: datum} : lastClickedNode"
        }
      ]
    },
    {
      "name": "mouseoverRectDatum",
      "description": "the datum associated with the node that is currently being moused over",
      "init": "null",
      "on": [
        {
          "events": "@row_clickable_rect:mouseover",
          "update": "isValid(datum) ? datum : null"
        },
        {"events": "@row_clickable_rect:mouseout", "update": "null"}
      ]
    },
    {
      "name": "verticalScrollbarMouseDown",
      "description": "boolean indicating whether the vertical scrollbar is currently being clicked",
      "value": false,
      "on": [
        {
          "events": "@group_verticalScrollbar:pointerdown",
          "update": "configVerticalScrollbar.enabled"
        },
        {
          "events": {
            "type": "mouseout",
            "scope": "view",
            "markname": "group_verticalScrollbar",
            "filter": ["!event.pointerdown"]
          },
          "update": "false"
        },
        {
          "events": {
            "type": "mouseover",
            "scope": "scope",
            "markname": "group_verticalScrollbar",
            "filter": ["event.pointerdown"]
          },
          "update": "true"
        },
        {"events": {"type": "pointerup"}, "update": "false"}
      ]
    }
,
    {
      "name": "verticalScrollbarMouseOver",
      "description": "boolean indicating whether the vertical scrollbar is currently being moused over",
      "value": false,
      "on": [
        {
          "events": "@group_verticalScrollbar:mouseover",
          "update": "configVerticalScrollbar.enabled"
        },
        {"events": "@group_verticalScrollbar:mouseout", "update": "false"}
      ]
    },
    {
      "name": "verticalScrollPercent",
      "description": "the percentage of the current vertical scroll",
      "value": 0,
      "on": [
        {
          "events": {
            "type": "wheel",
            "consume": true,
            "filter": ["!event.ctrlKey", "!event.shiftKey"]
          },
          "update": "!configVerticalScrollbar.enabled ? 0 : clamp(verticalScrollPercent - (-event.deltaY * pow(4, event.deltaMode) * 0.001), 0,(actualHeight-adjustedHeight)/actualHeight)"
        },
        {
          "events": "@group_verticalScrollbar:pointerdown",
          "update": "!configVerticalScrollbar.enabled ? 0 : invert('scaleScrollHandleY', y(group()))"
        },
        {
          "events": {
            "type": "pointermove",
            "source": "scope",
            "markname": "group_verticalScrollbar",
            "between": [{"type": "pointerdown"}, {"type": "pointerup"}]
          },
          "update": "!configVerticalScrollbar.enabled ? 0 : verticalScrollbarMouseDown && isValid(group()) ? invert('scaleScrollHandleY', clamp(y(group()), configVerticalScrollbar.handle.height,  configVerticalScrollbar.track.height)) : verticalScrollPercent"
        },
        {
          "events": {
            "type": "pointermove",
            "source": "scope",
            "markname": "group_verticalScrollbar",
            "between": [{"type": "pointerdown"}, {"type": "mouseout"}]
          },
          "update": "!configVerticalScrollbar.enabled ? 0 : isValid(group()) ? invert('scaleScrollHandleY', clamp(y(group()), configVerticalScrollbar.handle.height,  configVerticalScrollbar.track.height)) : verticalScrollPercent"
        },
        {
          "events": {"signal": "!configVerticalScrollbar.enabled"},
          "update": "0"
        }
      ]
    },
    {
      "name": "horizontalScrollbarMouseDown",
      "description": "boolean indicating whether the horizontal scrollbar is currently being clicked",
      "value": false,
      "on":  [
        {
          "events": "@group_horizontalScrollbar:pointerdown",
          "update": "configHorizontalScrollbar.enabled"
        },
        {
          "events": {
            "type": "mouseout",
            "scope": "view",
            "markname": "group_horizontalScrollbar",
            "filter": ["!event.pointerdown"]
          },
          "update": "false"
        },
        {
          "events": {
            "type": "mouseover",
            "scope": "scope",
            "markname": "group_horizontalScrollbar",
            "filter": ["event.pointerdown"]
          },
          "update": "true"
        },
        {"events": {"type": "pointerup"}, "update": "false"}
      ]
    },
    {
      "name": "horizontalScrollbarMouseOver",
      "description": "boolean indicating whether the horizontal scrollbar is currently being moused over",
      "value": false,
      "on": [
        {
          "events": "@group_horizontalScrollbar:mouseover",
          "update": "configHorizontalScrollbar.enabled"
        },
        {"events": "@group_horizontalScrollbar:mouseout", "update": "false"}
      ]
    },
    {
  "name": "horizontalScrollPercent",
  "value": 0,
  "on": [
    {
      "events": {
        "type": "wheel",
        "consume": true,
        "filter": ["event.shiftKey", "!event.ctrlKey"]
      },
      "update": "!configHorizontalScrollbar.enabled ? 0 : clamp(horizontalScrollPercent - (event.deltaY * pow(4, event.deltaMode) * 0.001), 0, ganttRange[1]-ganttRange[0])"
    },
    {
      "events": "@group_horizontalScrollbar:pointerdown",
      "update": "!configHorizontalScrollbar.enabled ? 0 : invert('scaleScrollHandleX', x(group()))"
    },
    {
      "events": {
        "type": "pointermove",
        "source": "scope",
        "markname": "group_horizontalScrollbar",
        "between": [{"type": "pointerdown"}, {"type": "pointerup"}]
      },
      "update": "!isValid(x(group())) ? horizontalScrollPercent : !configHorizontalScrollbar.enabled ? 0 : horizontalScrollbarMouseDown && isValid(group()) ? invert('scaleScrollHandleX', clamp(x(group()), configHorizontalScrollbar.handle.width, configHorizontalScrollbar.track.width)) : horizontalScrollPercent"
    },
    {
      "events": {"signal": "!configHorizontalScrollbar.enabled"},
      "update": "0"
    },
    {
      "events": "@reset_slider_granularity_interactivity_rect:pointerdown",
      "update": "scrollToTodayPercent"
    }
  ]
}
,
    {
  "name": "centerOnTodayClicked",
  "description": "Se activa al hacer click en el botón para centrar la vista en la línea HOY",
  "value": false,
  "on": [
    {
      "events": "@centerOnTodayButton:click",
      "update": "true"
    }
  ]
}
    ,
    {
      "name": "dateGranularitySliderMouseDown",
      "description": "boolean indicating whether the date granularity slider is currently being clicked",
      "value": false,
      "on": [
        {
          "events": "@dateGranularitySliderInteractive_rect:pointerdown",
          "update": "true"
        },
        {"events": "pointerup", "update": "false"}
      ]
    },
    {
      "name": "dateStepSizePercent",
      "init": "configDateStepSizePercentInital",
      "on": [
        {
          "events": "@dateGranularitySliderInteractive_rect:pointerdown",
          "update": "invert('scaleSliderHandleX', (x(group())-(width-configDateStepSlider.track.width)+(configVerticalScrollbar.enabled ? configVerticalScrollbar.innerPadding + configVerticalScrollbar.track.width  : 0)))"
        },
        {
          "events": {
            "type": "pointermove",
            "source": "scope",
            "markname": "dateGranularitySliderInteractive_rect",
            "between": [{"type": "pointerdown"}, {"type": "pointerup"}]
          },
          "update": "isValid(group()) ? invert('scaleSliderHandleX', clamp((x(group())-(width-configDateStepSlider.track.width)+(configVerticalScrollbar.enabled ? configVerticalScrollbar.innerPadding + configVerticalScrollbar.track.width  : 0)), configDateStepSlider.track.cornerRadius,  configDateStepSlider.track.width- configDateStepSlider.track.cornerRadius)) : dateStepSizePercent"
        },
        {
          "events": "@reset_slider_granularity_interactivity_rect:pointerdown",
          "update": "configDateStepSizePercentInital"
        },
        {
          "events": {
            "type": "wheel",
            "consume": true,
            "filter": ["event.ctrlKey", "!event.shiftKey"]
          },
          "update": "clamp(dateStepSizePercent - (-event.deltaY * 0.00025), 0,1)"
        }
      ]
    },
    {
      "name": "dateStepWidth",
      "description": "the step width for each band in the x-axis",
      "update": "scale('scaleStepWidth', dateStepSizePercent)"
    },
    {
      "name": "dateGranularity",
      "description": "a string indicating if the x-axis scale is currently set for days, months, or years",
      "update": "dateStepSizePercent< 0.33 ? 'day' : dateStepSizePercent < 0.66 ? 'month' : 'year' "
    },
    {
      "name": "dateStepDomain",
      "description": "percentage ranges mapped to date granularity (i.e. day, month, year)",
      "update": "[[0,0.33], [0.33, 0.66], [0.66,1]][indexof(['day','month','year'], dateGranularity)]"
    },
    {
      "name": "dateStepSliderResetHover",
      "description": "boolean indicating whether the date granularity reset button is currently being moused over",
      "value": false,
      "on": [
        {
          "events": [
            {
              "type": "mouseover",
              "markname": "reset_slider_granularity_interactivity_rect",
              "source": "scope"
            }
          ],
          "update": "true"
        },
        {
          "events": [
            {
              "type": "mouseout",
              "markname": "reset_slider_granularity_interactivity_rect",
              "source": "scope"
            }
          ],
          "update": "false"
        }
      ]
    },
    {
      "name": "showDetails",
      "description": "the currently toggled value for the 'show details' control",
      "init": "showDetailsConfig.initialValue",
      "on": [
        {
          "events": "@group_showDetailsToggle:pointerdown",
          "update": "!showDetails"
        }
      ]
    },
    {
      "name": "dayInMilliseconds",
      "description": "numeric value for 24 hours in milliseconds",
      "init": "8.64e+7"
    },
    {
  "name": "leftHandColumnsTotalWidth",
  "description": "total width of the columns on the left side before the Gantt",
  "update": "data('LeftHandColumns')[0]['totalWidth']"
}

,
    {
      "name": "firstVisibleRowId",
      "description": "the id of the first rendered row. This is used to determine if the 'Today' label should be hidden",
      "update": "(length(data('hierarchy_master'))*verticalScrollPercent)===0 ? 1 : round((length(data('hierarchy_master'))*verticalScrollPercent))+2"
    },
    {
      "name": "secondVisibleRowId",
      "description": "the id of the second rendered row. This is used to determine if the 'Today' label should be hidden",
      "update": "(length(data('hierarchy_master'))*verticalScrollPercent)===0 ? 1 : round((length(data('hierarchy_master'))*verticalScrollPercent))+3"
    },
    {
      "name": "progressXRange",
      "description": "the range used for the scale that determines the width of the progress bar in the 'Progress' column",
      "update": "[0, (pluck(data('LeftHandColumns'), 'allowableWidth')[indexof(pluck(data('LeftHandColumns'), 'column'), 'progress')])]"
    },
    {
      "name": "ganttDomain",
      "description": "the domain used in the x-axis scale for the Gantt",
      "update": "extent(pluck(data('date'), 'date'))"
    },
    {
  "name": "today",
  "update": "datetime(year(now()), month(now()), date(now()) - 1, 12, 0, 0)"
},{
  "name": "timeExtent",
  "update": "extent(data('ganttData'), 'time(datum.startDate)')"
}
,{
  "name": "scrollToTodayPercent",
  "description": "Centrar vista en HOY ±5 días dentro de ganttRange",
  "update": "clamp((time(today) - ganttRange[0] - 5*24*60*60*1000) / (ganttRange[1] - ganttRange[0]), 0, 1)",
  "on": [
    {
      "events": "@reset_slider_granularity_interactivity_rect:pointerdown",
      "update": "clamp((time(today) - ganttRange[0] - 5*24*60*60*1000) / (ganttRange[1] - ganttRange[0]), 0, 1)"
    }
  ]
}
,
    {
      "name": "ganttRange",
      "description": "the range used in the x-axis scale for the Gantt",
      "update": "[leftHandColumnsTotalWidth+10, width-(configVerticalScrollbar.enabled ? configVerticalScrollbar.innerPadding*2 : 0)]"
    },
    {
      "name": "padding",
      "description": "Padding around the visual canvas. This is updated dynamically to account for the vertical scrollbar when visible.",
      "update": "{top: 10, right: (configVerticalScrollbar.enabled ? 0 : configVerticalScrollbar.track.width), bottom: 10, left: 15}"
    },
    {
      "name": "width",
      "description": "the actual width of the visual",
      "update": "configDesiredTotalVisualWidth+1+(configVerticalScrollbar.enabled ? configVerticalScrollbar.track.width : 0)"
    },
    {
      "name": "adjustedHeight",
      "description": "The adjusted height excludes the last row. This is necessary to display the horizontal scroll bar",
      "update": "height - configRow.rowHeight"
    },
    {
      "name": "actualHeight",
      "description": "the height of all the rows if they were not being cut off by vertical scrolling",
      "update": "data('height')[0]['height']"
    }
  ],
  "marks": [
    {
      "description": "the line that sits under the column title marks. It also spans across the entire width of the visual and serves as the gantt x-axis domain",
      "name": "header_footer_divider_line",
      "type": "rect",
      "interactive": false,
      "encode": {
        "update": {
          "x": {"signal": "0"},
          "y": {"signal": "min(adjustedHeight, actualHeight)"},
          "x2": {"signal": "ganttRange[1]"},
          "height": {"signal": "0"},
          "cursor": {"value": "pointer"},
          "fill": {"value": "#6e7a87"},
          "stroke": {"value": "#6e7a87"},
          "strokeWidth": {"value": 0.1}
        }
      }
    },
    {
      "name": "column_header_group",
      "description": "the header marks for the lefthand columns",
      "type": "group",
      "marks": [
        {
          "name": "left_column_labels",
          "description": "column header text labels",
          "type": "text",
          "from": {"data": "LeftHandColumns"},
          "interactive": false,
          "encode": {
            "update": {
              "text": {"field": "label"},
              "x": {
                "signal": "datum.column==='name' ? configRow.levelIndentWidth : datum.x"
              },
              "y": {"value": -5},
              "baseline": {"value": "bottom"},
              "align": {"field": "align"},
              "fontSize": {"value": 18},
              "font": {"value": "Segoe UI"},
              "fontWeight": {"value": "600"}
            }
          }
        },
        {
          "name": "column_header_divider_line",
          "description": "the horizontal line that sits under the column labels",
          "type": "rect",
          "interactive": false,
          "encode": {
            "update": {
              "x": {"signal": "0"},
              "y": {"signal": "0"},
              "x2": {"signal": "ganttRange[1]"},
              "height": {"signal": "0"},
              "cursor": {"value": "pointer"},
              "fill": {"value": "#6e7a87"},
              "stroke": {"value": "#6e7a87"},
              "strokeWidth": {"value": 0.1}
            }
          }
        }
      ]
    },
    {
      "name": "ganttGrid_rect",
      "description": "a series of rects that serve as the Gantt's x-axis grid",
      "type": "rect",
      "from": {"data": "date"},
      "interactive": false,
      "encode": {
        "update": {
          "x": {"scale": "xBandScaleDate", "field": "date"},
          "width": {"signal": "bandwidth('xBandScaleDate')"},
          "y": {"signal": "0"},
          "height": {"signal": "extent([adjustedHeight, actualHeight])[0]"},
          "fill": {
            "signal": "dateGranularity === 'day' && indexof(['Sat', 'Sun'], utcFormat(datum.date, '%a')) >= 0 ? '#9ba5b2' : 'transparent'"
          },
          "fillOpacity": {"value": 0.05},
          "stroke": {"signal": "'#6e7a87'"},
          "strokeWidth": {"signal": "0.0115"}
        }
      }
    },
{
  "name": "dias_semana_labels",
  "type": "text",
  "from": {"data": "date"},
  "encode": {
    "update": {
      "text": {
        "signal": "utcFormat(datum.date, '%a')"
      },
      "x": {
        "scale": "xBandScaleDate",
        "field": "date",
        "offset": {"signal": "bandwidth('xBandScaleDate')/2"}
      },
      "y": {"value": -14},
      "align": {"value": "center"},
      "baseline": {"value": "top"},
      "fontSize": {"value": 13},
      "fill": {"value": "#666"}
    }
  }
},
    {
      "name": "today_line_rect",
      "description": "the vertical dashed line that indicates today's date in the gantt",
      "type": "rect",
      "from": {"data": "today"},
      "interactive": false,
      "encode": {
        "update": {
          "x": {"signal": "datum.x"},
          "x2": {"signal": "datum.x"},
          "y": {"signal": "0"},
          "y2": {"signal": "min(adjustedHeight, actualHeight)"},
          "fill": {"value": "transparent"},
          "stroke": {"signal": "configGantt.todayLine.stroke"},
          "strokeWidth": {"value": 2},
          "strokeDash": {"signal": "configGantt.todayLine.strokeDash"},
          "opacity": {"signal": "isValid(datum.x) ? 1 : 0"}
        }
      }
    },
    {
      "name": "group_row_level_marks",
      "description": "marks that are generated for each row",
      "type": "group",
      "clip": true,
      "encode": {
        "update": {
          "x": {"signal": "0"},
          "y": {
            "signal": "configVerticalScrollbar.enabled ? clamp(-verticalScrollPercent*actualHeight,-(actualHeight-adjustedHeight), 0) : 0"
          }
        }
      },
      "marks": [
        {
          "name": "row_clickable_rect",
          "description": "the invisible rect used for interactions for each row that spans across the visual horizontally",
          "type": "rect",
          "from": {"data": "hierarchy_master"},
          "interactive": true,
          "encode": {
            "update": {
              "x": {"signal": "0"},
              "y": {"signal": "configRow.rowHeight*datum['rowNumber']"},
              "width": {"signal": "ganttRange[1]"},
              "height": {
                "signal": "configVerticalScrollbar.enabled && datum.isLastRow ? 0 : configRow.rowHeight"
              },
              "cursor": {"signal": "datum.hasChildren ? 'pointer' : 'default'"},
              "fill": {"value": "#d1e0ec"},
              "fillOpacity": {
                "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 0.2 : 0"
              }
            }
          }
        },
        {
          "name": "divider_lines_group",
          "description": "group of marks that act as the diving lines of the visual",
          "type": "group",
          "marks": [
            {
              "name": "row_divider_lines",
              "description": "the horizontal divider lines that separate each row",
              "type": "rect",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {
                    "signal": "datum.indentWidth+(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'name')])"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*(datum['rowNumber']+1) - (datum.isLastRow ? 1 : 0)"
                  },
                  "x2": {"signal": "configGantt.x"},
                  "height": {"signal": "0"},
                  "cursor": {"value": "pointer"},
                  "fill": {"value": "#6e7a87"},
                  "stroke": {"value": "#6e7a87"},
                  "strokeWidth": {"signal": "datum.isLastRow ? 0 : 0.035"}
                }
              }
            },
            {
              "name": "column_lefthandColumn_divider_line",
              "description": "the far-left vertical line that acts as the lefthand border for the visual",
              "type": "rect",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"signal": "0"},
                  "y": {"signal": "configRow.rowHeight*(datum['rowNumber'])"},
                  "x2": {"signal": "0"},
                  "height": {"signal": "configRow.rowHeight"},
                  "cursor": {"value": "pointer"},
                  "fill": {"value": "#6e7a87"},
                  "stroke": {"value": "#6e7a87"},
                  "strokeWidth": {"value": 0.1},
                  "zindex": {"value": 99}
                }
              }
            },
            {
              "name": "column_gantt_divider_line",
              "description": "the vertical line that separates the columns and the Gantt",
              "type": "rect",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"signal": "configGantt.x"},
                  "y": {"signal": "configRow.rowHeight*(datum['rowNumber'])"},
                  "x2": {"signal": "configGantt.x"},
                  "height": {"signal": "configRow.rowHeight"},
                  "cursor": {"value": "pointer"},
                  "fill": {"value": "#6e7a87"},
                  "stroke": {"value": "#6e7a87"},
                  "strokeWidth": {"value": 0.1}
                }
              }
            },
            {
              "name": "gantt_end_divider_line",
              "description": "the far-right vertical line that acts as the righthand border for the visual",
              "type": "rect",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"signal": "ganttRange[1]"},
                  "y": {"signal": "configRow.rowHeight*(datum['rowNumber'])"},
                  "x2": {"signal": "ganttRange[1]"},
                  "height": {"signal": "configRow.rowHeight"},
                  "cursor": {"value": "pointer"},
                  "fill": {"value": "#6e7a87"},
                  "stroke": {"value": "#6e7a87"},
                  "strokeWidth": {"value": 0.1}
                }
              }
            }
          ]
        },
        {
          "name": "column_marks_group",
          "type": "group",
          "description": "all the marks that make up the lefthand columns",
          "marks": [
            {
              "name": "expand_collapse_indicator",
              "description": "the arrow indicator for parent nodes that indicate whether the current state of the node is expanded (arrow pointing down) or collapsed (arrow pointing to the right).",
              "type": "text",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {"signal": "!isValid(datum.isExpanded) ? '' : '➤'"},
                  "angle": {"field": "expandCollapseIndicatorAngle"},
                  "x": {"field": "expandCollapseIndicatorX"},
                  "y": {
                    "signal": "configRow.rowHeight*datum['rowNumber']+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configColumn.columns.taskTitle.font"},
                  "fontSize": {"signal": "12"},
                  "fill": {"signal": "datum.color"},
                  "fillOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 1 : 0.5"
                  },
                  "align": {"value": "center"},
                  "baseline": {"value": "middle"},
                  "opacity": {"field": "isVisible"},
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 800 : 400"
                  }
                }
              }
            },
            {
              "name": "task_title_text",
              "type": "text",
              "description": "The name of the node (i.e. task title)",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {
                    "signal": "indexof(configIncludedLeftHandColumns, 'name') >= 0 ? datum.name : ''"
                  },
                  "x": {
                    "signal": "datum.indentWidth+(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'name')])"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configColumn.columns.taskTitle.font"},
                  "fontSize": {
                    "signal": "configColumn.columns.taskTitle.fontSize"
                  },
                  "align": {
                    "signal": "(pluck(data('LeftHandColumns'), 'align')[indexof(pluck(data('LeftHandColumns'), 'column'), 'name')])"
                  },
                  "baseline": {"value": "middle"},
                  "limit": {
                    "signal": "configColumn.columns.taskTitle.allowableWidth"
                  },
                  "opacity": {"field": "isVisible"},
                  "fill": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#000' : '#666'"
                  },
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 700 : configColumn.columns.taskTitle.fontWeight"
                  }
                }
              }
            },
            {
              "name": "task_start_date_text",
              "description": "node's formatted start date",
              "type": "text",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {
                    "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'startDate') >= 0 ? utcFormat(datum.startDate, '%m/%d/%y') : ''"
                  },
                  "x": {
                    "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'startDate')])"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configColumn.columns.startDate.font"},
                  "fontSize": {
                    "signal": "configColumn.columns.startDate.fontSize"
                  },
                  "align": {
                    "signal": "(pluck(data('LeftHandColumns'), 'align')[indexof(pluck(data('LeftHandColumns'), 'column'), 'startDate')])"
                  },
                  "baseline": {"value": "middle"},
                  "limit": {
                    "signal": "configColumn.columns.startDate.allowableWidth"
                  },
                  "opacity": {"signal": "datum.isVisible && showDetails"},
                  "fill": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#000' : '#666'"
                  },
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 500 : 400"
                  }
                }
              }
            },
            {
              "name": "task_end_date",
              "type": "text",
              "description": "node's formatted end date",
              "from": {"data": "hierarchy_master"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {
                    "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'endDate') >= 0 ? utcFormat(datum.endDate, '%m/%d/%y') : ''"
                  },
                  "x": {
                    "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'endDate')])"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configColumn.columns.endDate.font"},
                  "fontSize": {
                    "signal": "configColumn.columns.endDate.fontSize"
                  },
                  "align": {
                    "signal": "(pluck(data('LeftHandColumns'), 'align')[indexof(pluck(data('LeftHandColumns'), 'column'), 'endDate')])"
                  },
                  "baseline": {"value": "middle"},
                  "limit": {
                    "signal": "configColumn.columns.endDate.allowableWidth"
                  },
                  "opacity": {"signal": "datum.isVisible && showDetails"},
                  "fill": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#000' : '#666'"
                  },
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 500 : 400"
                  }
                }
              }
            },
            {
  "name": "task_duration",
  "type": "text",
  "description": "calculated number of days between the node's start and end dates, EXCLUDING domingos",
  "from": {"data": "hierarchy_master"},
  "interactive": false,
  "encode": {
    "update": {
      "text": {
        "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'duration') >= 0 ? (round((datum.endDate-datum.startDate)/dayInMilliseconds) - (isValid(datum.domingos) ? datum.domingos : 0)) + ' d' : ''"
      },
      "x": {
        "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'duration')])"
      },
      "y": {
        "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2"
      },
      "font": {"signal": "configColumn.columns.duration.font"},
      "fontSize": {
        "signal": "configColumn.columns.duration.fontSize"
      },
      "align": {
        "signal": "(pluck(data('LeftHandColumns'), 'align')[indexof(pluck(data('LeftHandColumns'), 'column'), 'duration')])"
      },
      "baseline": {"value": "middle"},
      "limit": {
        "signal": "configColumn.columns.duration.allowableWidth"
      },
      "opacity": {"signal": "datum.isVisible && showDetails"},
      "fill": {
        "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#000' : '#666'"
      },
      "fontWeight": {
        "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 500 : 400"
      }
    }
  }
},
            {
              "type": "group",
              "name": "progress_column_container_group",
              "description": "group containig all the marks that make up the progress column",
              "marks": [
                {
                  "name": "progress_container_rect",
                  "type": "rect",
                  "from": {"data": "hierarchy_master"},
                  "interactive": false,
                  "encode": {
                    "update": {
                      "x": {
                        "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'progress')])"
                      },
                      "width": {
                        "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'progress') >= 0 ? (pluck(data('LeftHandColumns'), 'allowableWidth')[indexof(pluck(data('LeftHandColumns'), 'column'), 'progress')]) : 0"
                      },
                      "y": {"signal": "configRow.rowHeight*datum.rowNumber+4"},
                      "height": {"signal": "configRow.rowHeight-8"},
                      "cornerRadius": {
                        "signal": "configColumn.columns.progress.cornerRadius"
                      },
                      "opacity": {"signal": "datum.isVisible && showDetails"},
                      "stroke": {"signal": "datum.color"},
                      "strokeOpacity": {
                        "signal": "!showDetails ? 0 : isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 0.5 : 0.25"
                      },
                      "strokeWidth": {"value": 2}
                    }
                  }
                },
                {
                  "name": "progress_bar_rect",
                  "type": "rect",
                  "from": {"data": "hierarchy_master"},
                  "interactive": false,
                  "encode": {
                    "update": {
                      "x": {
                        "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'progress')])"
                      },
                      "width": {
                        "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'progress') >= 0 ? scale('scaleXProgress',datum.decimalPercentComplete) : 0"
                      },
                      "y": {"signal": "configRow.rowHeight*datum.rowNumber+4"},
                      "height": {"signal": "configRow.rowHeight-8"},
                      "cornerRadiusTopLeft": {
                        "signal": "configColumn.columns.progress.cornerRadius"
                      },
                      "cornerRadiusBottomLeft": {
                        "signal": "configColumn.columns.progress.cornerRadius"
                      },
                      "cornerRadiusTopRight": {
                        "signal": "datum.decimalPercentComplete === 1 ? configColumn.columns.progress.cornerRadius : 0"
                      },
                      "cornerRadiusBottomRight": {
                        "signal": "datum.decimalPercentComplete === 1 ? configColumn.columns.progress.cornerRadius : 0"
                      },
                      "opacity": {"signal": "datum.isVisible && showDetails"},
                      "fill": {"signal": "datum.color"},
                      "fillOpacity": {
                        "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 0.5 : 0.25"
                      }
                    }
                  }
                },
                {
                  "name": "progress_text",
                  "type": "text",
                  "from": {"data": "hierarchy_master"},
                  "interactive": false,
                  "encode": {
                    "update": {
                      "text": {
                        "signal": "showDetails && indexof(configIncludedLeftHandColumns, 'progress') >= 0 ? format(datum.decimalPercentComplete, '.0%') : ''"
                      },
                      "x": {
                        "signal": "(pluck(data('LeftHandColumns'), 'x')[indexof(pluck(data('LeftHandColumns'), 'column'), 'progress')])"
                      },
                      "dx": {"value": 5},
                      "y": {
                        "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2"
                      },
                      "font": {"signal": "configColumn.columns.duration.font"},
                      "fontSize": {
                        "signal": "configColumn.columns.progress.fontSize"
                      },
                      "baseline": {"value": "middle"},
                      "limit": {
                        "signal": "configColumn.columns.duration.allowableWidth"
                      },
                      "opacity": {"signal": "datum.isVisible && showDetails"},
                      "fill": {
                        "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#000' : '#666'"
                      },
                      "fontWeight": {
                        "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 500 : 400"
                      }
                    }
                  }
                }
              ]
            }
          ]
        },
        {
          "name": "group_dependencyLinks",
          "description": "group that makes up the dependency marks in the Gantt (i.e. arrowed lines)",
          "type": "group",
          "interactive": false,
          "from": {
            "facet": {
              "name": "dependencyLinksFacet",
              "data": "dependencyLinks",
              "groupby": ["sourceId", "targetId"]
            }
          },
          "marks": [
            {
              "name": "dependencyLinks_line",
              "description": "the dependency line mark",
              "type": "line",
              "from": {"data": "dependencyLinksFacet"},
              "encode": {
                "update": {
                  "x": {"signal": "datum.xy.x"},
                  "y": {"signal": "datum.xy.y"},
                  "stroke": {"value": "#666"},
                  "strokeWidth": {
                    "signal": "scale('scaleDependencyLinkStrokeWidth', datum.rowSeparationCount)"
                  },
                  "strokeOpacity": {
                    "signal": "scale('scaleDependencyLinkOpacity', datum.rowSeparationCount)"
                  },
                  "opacity": {"field": "opacity"},
                  "interpolate": {"value": "linear"},
                  "strokeJoin": {"value": "bevel"},
                  "strokeCap": {"value": "round"},
                  "defined": {"value": true}
                }
              }
            },
            {
              "name": "dependencyLinks_Arrow",
              "description": "the dependency arrow mark",
              "type": "text",
              "from": {"data": "dependencyLinksFacet"},
              "encode": {
                "update": {
                  "x": {"signal": "datum.xy.x"},
                  "dx": {"value": -3},
                  "y": {"signal": "datum.xy.y"},
                  "dy": {"value": 1},
                  "text": {"signal": "datum.sort === 6 ? '➤' : ''"},
                  "fillOpacity": {
                    "signal": "datum.sort === 6 ? scale('scaleDependencyLinkOpacity', datum.rowSeparationCount) : 0"
                  },
                  "opacity": {"field": "opacity"},
                  "fontSize": {"value": 8},
                  "align": {"value": "right"},
                  "baseline": {"value": "middle"},
                  "fill": {"value": "#888888"},
                  "stroke": {"value": "#888888"}
                }
              }
            }
          ]
        },
        {
          "name": "gantt_node_marks_group",
          "description": "The marks that make up the bars Gantt's bars",
          "type": "group",
          "marks": [
            {
              "name": "gantt_parent_rect_start_marker",
              "description": "the vertical line that appears at the start of parent node rects",
              "type": "rect",
              "from": {"data": "hierarchy_master_gantt_area"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"field": "x1", "offset": -1},
                  "x2": {"field": "x1", "offset": 0.5},
                  "y": {
                    "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight*0.25"
                  },
                  "height": {"signal": "configRow.rowHeight*0.15"},
                  "fill": {"signal": "datum.color"},
                  "opacity": {
                    "signal": "datum.shapeType === 'parentRect' && inrange(datum.startDate, domain('xBandScaleDate')) ? (datum.decimalPercentComplete > 0 ? 1 : 0.35) : 0"
                  }
                }
              }
            },
            {
              "name": "gantt_parent_rect_end_marker",
              "description": "the vertical line that appears at the end of parent node rects",
              "type": "rect",
              "from": {"data": "hierarchy_master_gantt_area"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"field": "x2", "offset": -1},
                  "x2": {"field": "x2", "offset": 0.5},
                  "y": {
                    "signal": "configRow.rowHeight*datum.rowNumber+configRow.rowHeight*0.25"
                  },
                  "height": {"signal": "configRow.rowHeight*0.5"},
                  "fill": {"signal": "datum.color"},
                  "opacity": {
                    "signal": "datum.shapeType === 'parentRect' && inrange(datum.endDate, domain('xBandScaleDate')) ? (datum.decimalPercentComplete === 1 ? 1 : 0.35) : 0"
                  }
                }
              }
            },
            {
              "name": "gantt_rect_container_background",
              "description": "a rect that matches the background color. This will sit behind the other rect marks, but will cover the grid marks and today-line mark",
              "type": "rect",
              "from": {"data": "hierarchy_master_gantt_area"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"field": "x1"},
                  "x2": {"signal": "datum.x2"},
                  "y": {
                    "signal": "datum.shapeType === 'parentRect' ? configRow.rowHeight*datum.rowNumber+configRow.rowHeight*0.25 : configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2 - configGantt.childRect.percentOfRowHeight*configRow.rowHeight/2"
                  },
                  "height": {
                    "signal": "(configRow.rowHeight)*(datum.shapeType === 'parentRect' ? configGantt.parentRect.percentOfRowHeight : configGantt.childRect.percentOfRowHeight)"
                  },
                  "cornerRadius": {
                    "signal": "datum.shapeType === 'parentRect' ? 0 : configGantt.childRect.cornerRadius"
                  },
                  "fill": {"signal": "background || 'transparent'"},
                  "fillOpacity": {"signal": "1"},
                  "strokeOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 0.2 : 1"
                  },
                  "stroke": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? '#d1e0ec' : background || '#fff'"
                  },
                  "strokeWidth": {"value": 0.99},
                  "opacity": {
                    "signal": "datum.shapeType === 'parentRect' || datum.shapeType === 'childRect' ? 1 : 0"
                  }
                }
              }
            },
            {
              "name": "gantt_rect_container",
              "type": "rect",
              "description": "semi-opaque gantt rect that will act as the background",
              "from": {"data": "hierarchy_master_gantt_area"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"signal": "datum.x1"},
                  "x2": {"signal": "datum.x2"},
                  "y": {
                    "signal": "datum.shapeType === 'parentRect' ? configRow.rowHeight*datum.rowNumber+configRow.rowHeight*0.25 : configRow.rowHeight*datum.rowNumber+configRow.rowHeight/2 - configGantt.childRect.percentOfRowHeight*configRow.rowHeight/2"
                  },
                  "height": {
                    "signal": "(configRow.rowHeight)*(datum.shapeType === 'parentRect' ? configGantt.parentRect.percentOfRowHeight : configGantt.childRect.percentOfRowHeight)"
                  },
                  "cornerRadius": {
                    "signal": "datum.shapeType === 'parentRect' ? 0 : configGantt.childRect.cornerRadius"
                  },
                  "fill": {"signal": "datum.color"},
                  "fillOpacity": {"value": 0.35},
                  "stroke": {"signal": "datum.color"},
                  "strokeWidth": {"value": 1},
                  "strokeOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.id ? 1 : 0.35"
                  },
                  "opacity": {
                    "signal": "datum.shapeType === 'parentRect' || datum.shapeType === 'childRect' ? 1 : 0"
                  }
                }
              }
            },
            {
              "name": "gantt_rect",
              "type": "rect",
              "description": "the rect that indicates percent complete",
              "from": {"data": "gantt_rect_container"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {"signal": "datum.bounds.x1"},
                  "x2": {"signal": "datum.datum.x2Progress"},
                  "y": {
                    "signal": "datum.datum.shapeType === 'parentRect' ? configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight*0.25 : configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight/2 - configGantt.childRect.percentOfRowHeight*configRow.rowHeight/2"
                  },
                  "height": {
                    "signal": "(configRow.rowHeight)*(datum.datum.shapeType === 'parentRect' ? configGantt.parentRect.percentOfRowHeight : configGantt.childRect.percentOfRowHeight)"
                  },
                  "cornerRadiusTopLeft": {
                    "signal": "datum.datum.shapeType === 'parentRect' ? 0 : configGantt.childRect.cornerRadius"
                  },
                  "cornerRadiusBottomLeft": {
                    "signal": "datum.datum.shapeType === 'parentRect' ? 0 : configGantt.childRect.cornerRadius"
                  },
                  "cornerRadiusTopRight": {
                    "signal": "datum.datum.shapeType === 'parentRect' ? 0 : (datum.datum.decimalPercentComplete === 1 ?  configGantt.childRect.cornerRadius : 0)"
                  },
                  "cornerRadiusBottomRight": {
                    "signal": "datum.datum.shapeType === 'parentRect' ? 0 :(datum.datum.decimalPercentComplete === 1 ? configGantt.childRect.cornerRadius : 0)"
                  },
                  "fill": {"signal": "datum.datum.color"},
                  "fillOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 1 : 0.35"
                  },
                  "opacity": {
                    "signal": "datum.datum.shapeType === 'parentRect' || datum.datum.shapeType === 'childRect' ? (datum.datum.decimalPercentComplete > 0 && (datum.datum.progressDate > domain('xBandScaleDate')[0]) ? 1 : 0) : 0"
                  },
                  "stroke": {"signal": "datum.datum.color"},
                  "strokeOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 1 : 0.35"
                  }
                }
              }
            },
            {
              "name": "gantt_milestone_background",
              "type": "symbol",
              "description": "the background for the symbol that represents a milestone",
              "from": {"data": "gantt_rect_container"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {
                    "signal": "(datum.datum.x2-datum.datum.x1)/2+datum.datum.x1+(dateGranularity === 'day' ? 0 : 0.35*configRow.rowHeight)"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight/2"
                  },
                  "shape": {"value": "diamond"},
                  "fill": {"signal": "background || '#fff'"},
                  "size": {"signal": "12*configRow.rowHeight"},
                  "fillOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 1 : 0.35"
                  },
                  "opacity": {
                    "signal": "datum.datum.shapeType === 'milestone' ? 1 : 0"
                  },
                  "stroke": {"signal": "background || '#fff'"},
                  "strokeOpacity": {"signal": "1"}
                }
              }
            },
            {
              "name": "gantt_milestone_symbol",
              "type": "symbol",
              "description": "the symbol that represents a milestone",
              "from": {"data": "gantt_rect_container"},
              "interactive": false,
              "encode": {
                "update": {
                  "x": {
                    "signal": "(datum.datum.x2-datum.datum.x1)/2+datum.datum.x1+(dateGranularity === 'day' ? 0 : 0.35*configRow.rowHeight)"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight/2"
                  },
                  "shape": {"value": "diamond"},
                  "fill": {"signal": "datum.datum.color"},
                  "size": {"signal": "12*configRow.rowHeight"},
                  "fillOpacity": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 1 : datum.datum.decimalPercentComplete === 1 ? 0.55 : 0.35"
                  },
                  "opacity": {
                    "signal": "datum.datum.shapeType === 'milestone' ? 1 : 0"
                  },
                  "stroke": {"signal": "background || '#fff'"},
                  "strokeOpacity": {"signal": "1"}
                }
              }
            },
            {
              "name": "gantt_label_text_background",
              "description": "the background for the node's text label. There is a thicker stroke to serve as an outline",
              "type": "text",
              "from": {"data": "gantt_rect_container"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {"signal": "datum.datum.label"},
                  "x": {"signal": "datum.datum.x2"},
                  "dx": {
                    "signal": "configGantt.label.xOffset*(datum.datum.shapeType === 'milestone' ? 2.5 : 1)"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configGantt.label.font"},
                  "fontSize": {"signal": "configGantt.label.fontSize"},
                  "align": {"value": "left"},
                  "baseline": {"value": "middle"},
                  "limit": {"signal": "ganttRange[1]-(datum.datum.x2+10)"},
                  "fill": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? '#f6f9fb' : background || '#fff'"
                  },
                  "fillOpacity": {
                    "signal": "!isValid(background) || background === '#fff' ? 1 : 0"
                  },
                  "strokeOpacity": {
                    "signal": "!isValid(background) || background === '#fff' ? 1 : 0"
                  },
                  "stroke": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? '#f6f9fb' : background || '#fff'"
                  },
                  "strokeWidth": {"value": 3},
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 800 : configGantt.label.fontWeight"
                  },
                  "opacity": {
                    "signal": "inrange(datum.datum.endDate, domain('xBandScaleDate')) ? 1 : 0"
                  }
                }
              }
            },
            {
              "name": "gantt_label_text",
              "description": "the node's text label",
              "type": "text",
              "from": {"data": "gantt_rect_container"},
              "interactive": false,
              "encode": {
                "update": {
                  "text": {"signal": "datum.datum.label"},
                  "x": {"signal": "datum.datum.x2"},
                  "dx": {
                    "signal": "configGantt.label.xOffset*(datum.datum.shapeType === 'milestone' ? 2.5 : 1)"
                  },
                  "y": {
                    "signal": "configRow.rowHeight*datum.datum.rowNumber+configRow.rowHeight/2"
                  },
                  "font": {"signal": "configGantt.label.font"},
                  "fontSize": {"signal": "configGantt.label.fontSize"},
                  "align": {"value": "left"},
                  "baseline": {"value": "middle"},
                  "limit": {"signal": "ganttRange[1]-(datum.datum.x2+10)"},
                  "fill": {"signal": "configGantt.label.fill"},
                  "fontWeight": {
                    "signal": "isValid(mouseoverRectDatum) && mouseoverRectDatum.id === datum.datum.id ? 600 : configGantt.label.fontWeight"
                  },
                  "opacity": {
                    "signal": "inrange(datum.datum.endDate, domain('xBandScaleDate')) ? 1 : 0"
                  }
                }
              }
            }
          ]
        }
      ]
    },
    {
  "name": "today_line_text",
  "description": "the text mark for the today line",
  "type": "text",
  "from": {"data": "today"},
  "interactive": true,
  "encode": {
    "update": {
      "tooltip": {"signal": "datum.labelOpacity"},
      "text": {"signal": "datum.labelOpacity === 0 ? '' : 'HOY'"},
      "x": {"signal": "datum.x"},
      "dx": {"signal": "5"},
      "y": {"signal": "0"},
      "dy": {"signal": "-5"},
      "angle": {"value": 90},
      "fill": {"signal": "configGantt.todayLine.stroke"},
      "fillOpacity": {"value": 1.75},
      "opacity": {
        "signal": "!isValid(datum.x) ? 0 : datum.labelOpacity"
      },
      "fontWeight": {"value": "bold"}
    }
  }
},
    {
      "name": "group_verticalScrollbar",
      "description": "the group of marks that make up the vertical scrollbar",
      "type": "group",
      "clip": true,
      "encode": {
        "update": {
          "x": {
            "signal": "verticalScrollbarMouseDown ? 0 : width-configVerticalScrollbar.track.width"
          },
          "width": {
            "signal": "configVerticalScrollbar.enabled ? verticalScrollbarMouseDown ? width+configVerticalScrollbar.track.width : configVerticalScrollbar.track.width : 0"
          },
          "y": {"value": 0},
          "height": {
            "signal": "configVerticalScrollbar.enabled ? configVerticalScrollbar.track.height : 0"
          },
          "fill": {"value": "transparent"},
          "cursor": {"value": "pointer"}
        }
      },
      "marks": [
        {
          "name": "rect_verticalScrollbar_track",
          "description": "the track for the scrollbar",
          "type": "rect",
          "interactive": false,
          "encode": {
            "update": {
              "x": {
                "signal": "verticalScrollbarMouseDown ? width-configVerticalScrollbar.track.width : 0"
              },
              "width": {
                "signal": "configVerticalScrollbar.enabled ? configVerticalScrollbar.track.width : 0"
              },
              "y": {"value": 0},
              "height": {
                "signal": "configVerticalScrollbar.enabled ? configVerticalScrollbar.track.height : 0"
              },
              "fill": {"signal": "configVerticalScrollbar.track.fill"},
              "cursor": {"value": "pointer"}
            }
          }
        },
        {
          "name": "rect_verticalScrollbar_handle",
          "description": "the handle for the scrollbar",
          "type": "rect",
          "interactive": false,
          "encode": {
            "update": {
              "x": {
                "signal": "verticalScrollbarMouseDown ? width-configVerticalScrollbar.track.width : 0"
              },
              "width": {
                "signal": "configVerticalScrollbar.enabled ? configVerticalScrollbar.track.width : 0"
              },
              "y": {
                "signal": "scale('scaleScrollHandleY', verticalScrollPercent)-configVerticalScrollbar.handle.height"
              },
              "y2": {
                "signal": "scale('scaleScrollHandleY', verticalScrollPercent)"
              },
              "fill": {
                "signal": "verticalScrollbarMouseOver ? configVerticalScrollbar.handle.hover.fill : configVerticalScrollbar.handle.fill"
              },
              "cursor": {"value": "pointer"}
            }
          }
        }
      ]
    },
    {
      "name": "last_row_concealer_rect",
      "description": "a rect that covers up the last node's row when the vertical scroll percentage is > 0 and less than 1. This is to allow space for the horizontal scrollbar when needed.",
      "type": "rect",
      "interactive": true,
      "encode": {
        "update": {
          "x": {"value": 0},
          "width": {"signal": "width"},
          "y": {"signal": "extent([actualHeight, adjustedHeight])[0]"},
          "height": {"signal": "configRow.rowHeight"},
          "fill": {"signal": "isValid(background) ? background : '#fff'"}
        }
      }
    },
    {
      "name": "group_horizontalScrollbar",
      "description": "the group of marks that make up the horizontal scrollbar",
      "type": "group",
      "clip": true,
      "zindex": 99,
      "interactive": true,
      "encode": {
        "update": {
          "y": {
            "signal": "horizontalScrollbarMouseDown ? 0 : extent([actualHeight, adjustedHeight])[0] +configHorizontalScrollbar.innerPadding"
          },
          "height": {
            "signal": "horizontalScrollbarMouseDown ? height : configHorizontalScrollbar.enabled ? +configHorizontalScrollbar.track.height : 0"
          },
          "x": {"signal": "ganttRange[0]"},
          "width": {
            "signal": "horizontalScrollbarMouseDown ? width :ganttRange[1]-ganttRange[0]"
          },
          "fill": {"value": "transparent"},
          "cursor": {"value": "pointer"}
        }
      },
      "marks": [
        {
          "name": "rect_horizontalScrollbar_track",
          "description": "the track for the scrollbar",
          "type": "rect",
          "interactive": false,
          "encode": {
            "update": {
              "y": {
                "signal": "!horizontalScrollbarMouseDown ? 0 : extent([actualHeight, adjustedHeight])[0] +configHorizontalScrollbar.innerPadding"
              },
              "height": {
                "signal": "configHorizontalScrollbar.enabled ? configHorizontalScrollbar.track.height : 0"
              },
              "width": {"signal": "ganttRange[1]-ganttRange[0]"},
              "fill": {"signal": "configHorizontalScrollbar.track.fill"},
              "cursor": {"value": "pointer"}
            }
          }
        },
        {
          "name": "rect_horizontalScrollbar_handle",
          "description": "the handle for the scrollbar",
          "type": "rect",
          "interactive": false,
          "encode": {
            "update": {
              "y": {
                "signal": "!horizontalScrollbarMouseDown ? 0 : extent([actualHeight, adjustedHeight])[0] +configHorizontalScrollbar.innerPadding"
              },
              "height": {
                "signal": "configHorizontalScrollbar.enabled ? configHorizontalScrollbar.track.height : 0"
              },
              "x": {
                "signal": "scale('scaleScrollHandleX', horizontalScrollPercent)-configHorizontalScrollbar.handle.width"
              },
              "x2": {
                "signal": "scale('scaleScrollHandleX', horizontalScrollPercent)"
              },
              "fill": {
                "signal": "horizontalScrollbarMouseOver ? configHorizontalScrollbar.handle.hover.fill : configHorizontalScrollbar.handle.fill"
              },
              "cursor": {"value": "pointer"}
            }
          }
        }
      ]
    },
    {
      "name": "group_header_marks",
      "description": "group of marks that make up the header controls",
      "type": "group",
      "encode": {
        "update": {"y": {"signal": "configHeader.yOffset-configHeader.height"}}
      },
      "marks": [
        {
          "name": "group_expand_all_collapse_all_buttons",
          "description": "the group of marks that make up the expand/collapse buttons",
          "type": "group",
          "marks": [
            {
              "name": "labelExpandCollapse_text",
              "description": "the title for the control",
              "type": "text",
              "interactive": false,
              "encode": {
                "update": {
                  "x": {
                    "signal": "data('expandAllCollapseAllConfig')[0]['rowNumber']*(12000*data('expandAllCollapseAllConfig')[0]['size'])",
                    "offset": {"signal": "configRow.levelIndentWidth"}
                  },
                  "text": {"signal": "configButtons.label.text || ''"},
                  "baseline": {"value": "top"},
                  "font": {"signal": "configButtons.label.font"},
                  "fontSize": {"signal": "configButtons.label.fontSize"},
                  "fontStyle": {"signal": "configButtons.label.fontStyle"},
                  "align": {"value": "left"},
                  "fill": {"signal": "configButtons.label.fill"}
                }
              }
            },
            {
              "name": "expandAll_collapseAll_dummy_button_labels",
              "description": "icon path marks that will be used by other button marks for reactive geometry",
              "type": "symbol",
              "from": {"data": "expandAllCollapseAllConfig"},
              "interactive": false,
              "encode": {
                "enter": {
                  "x": {
                        "signal": "configButtons.padding + datum.rowNumber * 30",
                        "offset": {"signal": "configRow.levelIndentWidth"}
                      },
                  "y": {
                    "signal": "configButtons.yOffset+configButtons.label.dy*1.3"
                  },
                  "size": {"field": "size"},
                  "shape": {"field": "path"},
                  "fill": {"value": "#999"}
                },
                "update": {
                  "fill": {
                    "signal": "isValid(expandCollapseButtonDatum) && expandCollapseButtonDatum.rowNumber === datum.rowNumber ? 'steelblue' : '#999'"
                  }
                }
              }
            },
            {
              "name": "expandAll_collapseAll_button_background",
              "description": "the background for the expand/collapse buttons",
              "type": "rect",
              "from": {"data": "expandAll_collapseAll_dummy_button_labels"},
              "interactive": true,
              "encode": {
                "enter": {
                  "x": {"signal": "datum.bounds.x1-configButtons.padding"},
                  "x2": {"signal": "datum.bounds.x2+configButtons.padding"},
                  "y": {"signal": "datum.bounds.y1-configButtons.padding"},
                  "y2": {"signal": "datum.bounds.y2+configButtons.padding"},
                  "cornerRadiusTopLeft": {
                    "signal": "datum.datum.rowNumber == 0 ? 5 : 0"
                  },
                  "cornerRadiusBottomLeft": {
                    "signal": "datum.datum.rowNumber == 0 ? 5 : 0"
                  },
                  "cornerRadiusTopRight": {
                    "signal": "datum.datum.rowNumber == 1 ? 5 : 0"
                  },
                  "cornerRadiusBottomRight": {
                    "signal": "datum.datum.rowNumber == 1 ? 5 : 0"
                  },
                  "fillOpacity": {"value": 1},
                  "stroke": {"signal": "background || '#fff'"},
                  "strokeWidth": {"value": 1},
                  "fill": {"signal": "background || '#fff'"},
                  "opacity": {"value": 1},
                  "cursor": {"value": "pointer"},
                  "tooltip": {"signal": "datum.datum.tooltip"}
                }
              }
            },
            {
              "name": "expandAll_collapseAll_buttons",
              "description": "the symbols that appear on the button faces",
              "type": "rect",
              "from": {"data": "expandAll_collapseAll_dummy_button_labels"},
              "interactive": true,
              "encode": {
                "enter": {
                  "x": {"signal": "datum.bounds.x1-configButtons.padding"},
                  "x2": {"signal": "datum.bounds.x2+configButtons.padding"},
                  "y": {"signal": "datum.bounds.y1-configButtons.padding"},
                  "y2": {"signal": "datum.bounds.y2+configButtons.padding"},
                  "cornerRadiusTopLeft": {
                    "signal": "datum.datum.rowNumber == 0 ? 5 : 0"
                  },
                  "cornerRadiusBottomLeft": {
                    "signal": "datum.datum.rowNumber == 0 ? 5 : 0"
                  },
                  "cornerRadiusTopRight": {
                    "signal": "datum.datum.rowNumber == 1 ? 5 : 0"
                  },
                  "cornerRadiusBottomRight": {
                    "signal": "datum.datum.rowNumber == 1 ? 5 : 0"
                  },
                  "fillOpacity": {"value": 0.25},
                  "stroke": {"signal": "configButtons.outerStroke"},
                  "strokeWidth": {"value": 1},
                  "cursor": {"value": "pointer"},
                  "tooltip": {"signal": "datum.datum.tooltip"}
                },
                "update": {
                  "fill": {
                    "signal": "isValid(expandCollapseButtonDatum) && expandCollapseButtonDatum.rowNumber === datum.datum.rowNumber ? configButtons.hoverFill : configButtons.fill"
                  }
                }
              }
            },
            {
              "name": "expandAll_collapseAll_button_labels",
              "description": "the actual button face symbols",
              "type": "symbol",
              "from": {"data": "expandAll_collapseAll_dummy_button_labels"},
              "interactive": true,
              "encode": {
                "enter": {
                  "x": {"signal": "datum.x"},
                  "y": {"signal": "datum.y"},
                  "size": {"field": "size"},
                  "shape": {"signal": "datum.datum.path"},
                  "fill": {"field": "fill"}
                },
                "update": {
                  "fill": {
                    "signal": "isValid(expandCollapseButtonDatum) && expandCollapseButtonDatum.rowNumber === datum.datum.rowNumber ? 'steelblue' : '#999'"
                  }
                }
              }
            }
          ]
        },
        {
          "name": "group_showDetailsToggle",
          "description": "group for the marks that make up the toggle control to hide/show details",
          "type": "group",
          "interactive": true,
          "from": {"data": "group_expand_all_collapse_all_buttons"},
          "clip": false,
          "encode": {
            "update": {
              "y": {"value": 0},
              "height": {"signal": "configHeader.height"},
              "x": {"signal": "datum.bounds.x2+showDetailsConfig.xOffset"},
              "width": {"signal": "showDetailsConfig.track.width"},
              "fill": {"value": "transparent"},
              "tooltip": {"signal": "showDetailsConfig.tooltip.text"},
              "cursor": {"value": "pointer"}
            }
          },
          "marks": [
            {
              "name": "showDetailsToggle_text",
              "description": "the title for the toggle control",
              "type": "text",
              "interactive": false,
              "encode": {
                "update": {

                  
                  "text": {"signal": "showDetailsConfig.label.text || ''"},
                  "baseline": {"value": "top"},
                  "font": {"signal": "showDetailsConfig.label.font"},
                  "fontSize": {"signal": "showDetailsConfig.label.fontSize"},
                  "fontStyle": {"signal": "showDetailsConfig.label.fontStyle"},
                  "align": {"value": "left"},
                  "fill": {"signal": "showDetailsConfig.label.fill"}
                }
              }
            },
            {
              "name": "track_rect",
              "description": "the track for the toggle control",
              "type": "rect",
              "from": {"data": "showDetailsToggle_text"},
              "interactive": false,
              "encode": {
                "update": {
                  "y": {"signal": "datum.bounds.y2+showDetailsConfig.label.dy"},
                  "height": {"signal": "showDetailsConfig.track.height"},
                  "x": {"signal": "0"},
                  "x2": {"signal": "showDetailsConfig.track.width"},
                  "cornerRadius": {
                    "signal": "showDetailsConfig.track.cornerRadius"
                  },
                  "fill": {
                    "signal": "showDetails ? showDetailsConfig.on.fill : showDetailsConfig.track.fill"
                  },
                  "stroke": {"signal": "showDetailsConfig.track.stroke"},
                  "strokeWidth": {
                    "signal": "showDetailsConfig.track.strokeWidth"
                  }
                }
              }
            },
            {
              "name": "toggle_outer_arc",
              "description": "the circle mark that serves as the the 'toggle handle'",
              "from": {"data": "showDetailsToggle_text"},
              "type": "arc",
              "interactive": false,
              "encode": {
                "enter": {
                  "y": {
                    "signal": "datum.bounds.y2+showDetailsConfig.label.dy+showDetailsConfig.track.height/2"
                  },
                  "innerRadius": {"value": 0},
                  "outerRadius": {
                    "signal": "showDetailsConfig.track.height*0.9"
                  },
                  "startAngle": {"signal": "0"},
                  "endAngle": {"signal": "2*PI"},
                  "stroke": {
                    "signal": "showDetailsConfig.handle.stroke || '#BBB'"
                  },
                  "strokeWidth": {
                    "signal": "showDetailsConfig.handle.strokeWidth"
                  },
                  "fill": {"signal": "showDetailsConfig.handle.fill || '#fff'"}
                },
                "update": {
                  "x": {
                    "signal": "showDetails ? showDetailsConfig.track.width-showDetailsConfig.track.cornerRadius : showDetailsConfig.track.cornerRadius"
                  }
                }
              }
            }
          ]
        },
        {
          "name": "group_dateGranularitySlider",
          "description": "the group of marks that makes up the slider control",
          "type": "group",
          "interactive": false,
          "clip": false,
          "encode": {
            "update": {
              "y": {"value": 0},
              "height": {"signal": "configHeader.height"},
              "x": {
                "signal": "width-configDateStepSlider.track.width - (configVerticalScrollbar.enabled ? configVerticalScrollbar.track.width + configVerticalScrollbar.innerPadding : 0)"
              },
              "x2": {"signal": "width"}
            }
          },
          "marks": [
            {
              "name": "labelDateGranularitySlider_text",
              "description": "the title for the slider control",
              "type": "text",
              "interactive": false,
              "encode": {
                "update": {
                  "text": {"signal": "configDateStepSlider.label.text || ''"},
                  "baseline": {"value": "top"},
                  "font": {"signal": "configDateStepSlider.label.font"},
                  "fontSize": {"signal": "configDateStepSlider.label.fontSize"},
                  "fontStyle": {
                    "signal": "configDateStepSlider.label.fontStyle"
                  },
                  "align": {"value": "left"},
                  "dx": {"signal": "-configDateStepSlider.track.cornerRadius"},
                  "fill": {"signal": "configDateStepSlider.label.fill"}
                }
              }
            },
            {
              "name": "track_rect",
              "description": "the track for the slider control",
              "type": "rect",
              "from": {"data": "labelDateGranularitySlider_text"},
              "interactive": false,
              "encode": {
                "update": {
                  "y": {
                    "signal": "datum.bounds.y2+configDateStepSlider.label.dy"
                  },
                  "height": {"signal": "configDateStepSlider.track.height"},
                  "x": {"signal": "0"},
                  "x2": {"signal": "configDateStepSlider.track.width"},
                  "cornerRadius": {
                    "signal": "configDateStepSlider.track.cornerRadius"
                  },
                  "fill": {"signal": "configDateStepSlider.track.fill"},
                  "stroke": {"signal": "configDateStepSlider.track.stroke"},
                  "strokeWidth": {
                    "signal": "configDateStepSlider.track.strokeWidth"
                  }
                }
              }
            },
            {
              "name": "sliderPercentage_rect",
              "description": "the rect that indicates the slider percentage",
              "type": "rect",
              "from": {"data": "labelDateGranularitySlider_text"},
              "interactive": false,
              "encode": {
                "update": {
                  "y": {
                    "signal": "datum.bounds.y2+configDateStepSlider.label.dy"
                  },
                  "height": {"signal": "configDateStepSlider.track.height"},
                  "x": {"value": 0},
                  "x2": {
                    "signal": "scale('scaleSliderHandleX', dateStepSizePercent)"
                  },
                  "cornerRadius": {
                    "signal": "configDateStepSlider.track.cornerRadius"
                  },
                  "fill": {"signal": "configDateStepSlider.progress.fill"},
                  "fillOpacity": {
                    "signal": "configDateStepSlider.progress.fillOpacity"
                  },
                  "stroke": {"signal": "configDateStepSlider.track.stroke"},
                  "strokeWidth": {
                    "signal": "configDateStepSlider.track.strokeWidth"
                  }
                }
              }
            },
            {
              "name": "reset_slider_granularity_symbol",
              "description": "circle with arrow symbol",
              "type": "symbol",
              "from": {"data": "track_rect"},
              "encode": {
                "update": {
                  "shape": {"signal": "configDateStepSlider.reset.iconPath"},
                  "size": {"signal": "0.0025"},
                  "x": {"signal": "datum.bounds.x1-20"},
                  "y": {"signal": "datum.bounds.y1-2.5"},
                  "fill": {
                    "signal": "dateStepSliderResetHover ? configDateStepSlider.reset.hoverFill : configDateStepSlider.reset.fill"
                  }
                }
              }
            },
            {
              "name": "reset_slider_granularity_interactivity_rect",
              "description": "the interactive rect that sits behind the reset icon",
              "type": "rect",
              "from": {"data": "reset_slider_granularity_symbol"},
              "encode": {
                "update": {
                  "x": {"signal": "datum.bounds.x1"},
                  "x2": {"signal": "datum.bounds.x2"},
                  "y": {"signal": "datum.bounds.y1"},
                  "y2": {"signal": "datum.bounds.y2"},
                  "fill": {"value": "transparent"},
                  "tooltip": {
                    "signal": "configDateStepSlider.reset.tooltipText"
                  },
                  "cursor": {"value": "pointer"}
                }
              }
            },
            {
              "name": "dateGranularitySliderInteractive_rect",
              "description": "the invisible rect that is used for capturing events related to the control",
              "from": {"data": "labelDateGranularitySlider_text"},
              "type": "rect",
              "interactive": true,
              "encode": {
                "update": {
                  "y": {
                    "signal": "dateGranularitySliderMouseDown ? 0 : (datum.bounds.y2+configDateStepSlider.label.dy)"
                  },
                  "x": {
                    "signal": "dateGranularitySliderMouseDown ? -(ganttRange[1]-ganttRange[0])/2 : 0"
                  },
                  "width": {
                    "signal": "configDateStepSlider.track.width + (dateGranularitySliderMouseDown ? (2*configDateStepSlider.track.strokeWidth+padding.right)+(ganttRange[1]-ganttRange[0])/2 : 0) + (configVerticalScrollbar.enabled ? configVerticalScrollbar.innerPadding + configVerticalScrollbar.track.width : 0)"
                  },
                  "height": {
                    "signal": "dateGranularitySliderMouseDown ? adjustedHeight : configDateStepSlider.track.height"
                  },
                  "fill": {"value": "transparent"},
                  "cursor": {"value": "pointer"},
                  "tooltip": {
                    "signal": "dateGranularitySliderMouseDown ? '' : configDateStepSlider.tooltip.text"
                  }
                }
              }
            },
            {
              "name": "handles_outer_arc",
              "description": "the outer circle mark that serves as the the 'slider handle'",
              "from": {"data": "labelDateGranularitySlider_text"},
              "type": "arc",
              "interactive": false,
              "encode": {
                "enter": {
                  "y": {
                    "signal": "datum.bounds.y2+configDateStepSlider.label.dy + configDateStepSlider.track.height/2"
                  },
                  "innerRadius": {"value": 0},
                  "outerRadius": {
                    "signal": "configDateStepSlider.track.height*0.9"
                  },
                  "startAngle": {"signal": "0"},
                  "endAngle": {"signal": "2*PI"},
                  "stroke": {
                    "signal": "configDateStepSlider.handle.outerStroke || '#BBB'"
                  },
                  "strokeWidth": {
                    "signal": "configDateStepSlider.handle.outerStrokeWidth"
                  },
                  "fill": {
                    "signal": "configDateStepSlider.handle.outerFill || '#fff'"
                  }
                },
                "update": {
                  "x": {
                    "signal": "scale('scaleSliderHandleX', dateStepSizePercent)"
                  }
                }
              }
            },
            {
              "name": "handles_inner_arc",
              "description": "the inner circle mark that serves as the the 'slider handle'",
              "type": "arc",
              "from": {"data": "labelDateGranularitySlider_text"},
              "interactive": false,
              "encode": {
                "enter": {
                  "y": {
                    "signal": "datum.bounds.y2+configDateStepSlider.label.dy + configDateStepSlider.track.height/2"
                  },
                  "innerRadius": {"value": 0},
                  "outerRadius": {"signal": "1"},
                  "startAngle": {"signal": "0"},
                  "endAngle": {"signal": "2*PI"},
                  "stroke": {
                    "signal": "configDateStepSlider.handle.innerStroke"
                  },
                  "fill": {
                    "signal": "configDateStepSlider.handle.innerFill || '#999'"
                  }
                },
                "update": {
                  "x": {
                    "signal": "scale('scaleSliderHandleX', dateStepSizePercent)"
                  }
                }
              }
            }
          ]
        }
      ]
    },
    {
      "name": "info_icon_symbol",
      "type": "symbol",
      "interactive": true,
      "encode": {
        "update": {
          "x": {"signal": "configInfo.x"},
          "y": {"signal": "configInfo.y"},
          "shape": {"signal": "configInfo.iconPath"},
          "size": {"signal": "configInfo.size"},
          "fill": {"signal": "configInfo.fill"},
          "tooltip": {"signal": "showInfoIcon ? configInfo.tooltip : null"},
          "cursor": {"signal": "showInfoIcon ? 'pointer' : 'default'"},
          "opacity": {"signal": "showInfoIcon ? 1 : 0"}
        },
        "hover": {
          "fill": {"signal": "configInfo.hoverFill"}
        }
      }
    }
  ],
  "scales": [
    {
      "name": "scaleXProgress",
      "type": "linear",
      "domain": [0, 1],
      "range": {"signal": "progressXRange"},
      "clamp": true
    },
    {
      "name": "scaleScrollHandleY",
      "type": "linear",
      "domain": [0, {"signal": "(actualHeight-adjustedHeight)/actualHeight"}],
      "range": {
        "signal": "[configVerticalScrollbar.handle.height, configVerticalScrollbar.track.height]"
      },
      "clamp": true
    },
    {
      "name": "scaleScrollHandleX",
      "type": "linear",
      "domain": [0, 1],
      "range": {
        "signal": "[configHorizontalScrollbar.handle.width, configHorizontalScrollbar.track.width]"
      },
      "clamp": true
    },
    {
      "name": "scaleSliderHandleX",
      "type": "linear",
      "domain": [0, 1],
      "range": {
        "signal": "[configDateStepSlider.track.width-configDateStepSlider.track.cornerRadius, configDateStepSlider.track.cornerRadius]"
      },
      "clamp": true
    },
    {
      "name": "xBandScaleDate",
      "type": "band",
      "domain": {"data": "date", "fields": ["date"], "sort": true},
      "range": {"signal": "ganttRange"}
    },
    {
      "name": "minDateRow",
      "type": "linear",
      "domain": [0, 1],
      "clamp": true,
      "range": {
        "signal": "[1, data('dateWindowSize')[0]['actualEndRowNumber'] -  data('dateWindowSize')[0]['windowRowCount']]"
      }
    },
    {
      "name": "maxDateRow",
      "type": "linear",
      "domain": [0, 1],
      "clamp": true,
      "range": {
        "signal": "[data('dateWindowSize')[0]['windowRowCount'], data('dateWindowSize')[0]['actualEndRowNumber']]"
      }
    },
    {
      "name": "scaleStepWidth",
      "type": "linear",
      "domain": {"signal": "dateStepDomain"},
      "clamp": true,
      "zero": false,
      "range": {"signal": "configDateStepRange"}
    },
    {
      "name": "scaleDependencyLinkOpacity",
      "domain": {
        "signal": "[1, extent(pluck(data('dependencyLinks'), 'rowSeparationCount'))[1]]"
      },
      "range": [0.5, 1]
    },
    {
      "name": "scaleDependencyLinkStrokeWidth",
      "domain": {
        "signal": "[1, extent(pluck(data('dependencyLinks'), 'rowSeparationCount'))[1]]"
      },
      "range": [0.5, 1]
    }
  ],
  "axes": [
    {
      "description": "date axis",
      "scale": "xBandScaleDate",
      "orient": "top",
      "offset": 2,
      "domain": false,
      "labelOverlap": false,
      "labelFontSize": {"value": 11},
      "labelFont": {"value": "Segoe UI"},
      "labelFontWeight": {"value": "600"},
      "labelLineHeight": 18,
      "labelPadding": 0,
      "tickSize": {
        "signal": "dateGranularity === 'day' ? utcFormat(datum.label, '%m-%d') === '01-01' ? 56 : utcFormat(datum.label, '%d') === '01' ? 38 : 20 : dateGranularity === 'month' ? utcFormat(datum.label, '%m-%d') === '01-01' ? 38 : 20 : 20"
      },
      "tickColor": {"value": "#6e7a87"},
      "tickWidth": {"signal": "0.25"},
      "labelOffset": {"signal": "bandwidth('xBandScaleDate')/2"},
      "tickOffset": {"signal": "-bandwidth('xBandScaleDate')/2"},
      "bandPosition": 0.5,
      "encode": {
  "labels": {
    "update": {
      "text": {
        "signal": "dateGranularity === 'day' ? indexof(pluck(data('date'), 'yearFormatted'), utcFormat(datum.label, '%Y')) >= 0 ? utcFormat(datum.label, '%m-%d') === '01-01' ? ['‎ ‎ ‎ ‎ '+utcFormat(datum.label, '%Y'), '‎ ‎ '+utcFormat(datum.label, '%b'), utcFormat(datum.label, '%d')] : utcFormat(datum.label, '%d') === '01' ? ['‎ ‎ '+utcFormat(datum.label, '%b'), utcFormat(datum.label, '%d')] : utcFormat(datum.label, '%d') : null : dateGranularity === 'month' ? utcFormat(datum.label, '%m-%d') === '01-01' ? ['‎ ‎'+utcFormat(datum.label, '%Y'), utcFormat(datum.label, '%b')] : utcFormat(datum.label, '%b') : utcFormat(datum.label, '%Y')"
      }
    }
  }
}
    }
  ],
  "data": [
    {
      "name": "dataset",
      "url": "https://raw.githubusercontent.com/Giammaria/PublicFiles/master/data/20240511_hierarchical_gantt_dataset.json",
      "format": {
        "parse": {
          "id": "number",
          "parentId": "number",
          "name": "string",
          "startDate": "date",
          "endDate": "date",
          "decimalPercentComplete": "number",
          "dependencyId": "string"
        }
      },
      "transform": []
    },
    {
      "name": "dataset_formatted",
      "source": "dataset",
      "transform": [
        {
          "type": "formula",
          "expr": "isValid(datum.color) ? datum.color : configRow.defaultFill",
          "as": "color"
        },
        {
          "type": "formula",
          "expr": "toDate(utcFormat(datum.startDate, '%Y-%m-%d'))",
          "as": "startDate"
        },
        {
          "type": "formula",
          "expr": "toDate(utcFormat(datum.endDate, '%Y-%m-%d'))",
          "as": "endDate"
        },
        {"type": "window", "ops": ["row_number"], "as": ["sort"]},
        {
          "type": "formula",
          "expr": "!isValid(datum['startDate']) ? null : datum['endDate']",
          "as": "endDate"
        },
        {"type": "stratify", "key": "id", "parentKey": "parentId"},
        {
          "type": "formula",
          "expr": "{id: datum.id, parentId: datum.parentId}",
          "as": "idObj"
        }
      ]
    },
    {
      "name": "allDates",
      "values": "[{}]",
      "transform": [
        {
          "type": "formula",
          "expr": "extent(pluck(data('dataset_formatted'), 'startDate'))[0]- (dayInMilliseconds*([1,31,2190][indexof(['day','month','year'], dateGranularity)]))",
          "as": "minDate"
        },
        {
          "type": "formula",
          "expr": "extent(pluck(data('dataset_formatted'), 'endDate'))[1]+ (dayInMilliseconds*([1,31,2190][indexof(['day','month','year'], dateGranularity)]))",
          "as": "maxDate"
        },
        {
          "type": "formula",
          "expr": "sequence(datum.minDate, datum.maxDate+dayInMilliseconds, dayInMilliseconds)",
          "as": "date"
        },
        {"type": "flatten", "fields": ["date"]},
        {
          "type": "formula",
          "expr": "utcFormat(datum.date, '%Y-%m-%d')",
          "as": "dateKey"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.date, '%Y-%m-%d')",
          "as": "dateFormatted"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.date, '%y-%m')",
          "as": "monthYearFormatted"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.date, '%Y')",
          "as": "yearFormatted"
        },
        {
          "type": "window",
          "ops": ["dense_rank"],
          "fields": ["dateFormatted"],
          "sort": {"field": "date"},
          "frame": [null, null],
          "as": ["dateDR"]
        },
        {
          "type": "window",
          "ops": ["dense_rank"],
          "fields": ["monthYearFormatted"],
          "sort": {"field": "monthYearFormatted"},
          "frame": [null, null],
          "as": ["monthDR"]
        },
        {
          "type": "window",
          "ops": ["dense_rank"],
          "fields": ["yearFormatted"],
          "sort": {"field": "yearFormatted"},
          "frame": [null, null],
          "as": ["yearDR"]
        }
      ]
    },
    {
  "name": "monthLookup",
  "values": [
    {"monthAbbrev": "Jan", "daysInMonth": 31, "monthAbbrevEs": "Ene"},
    {"monthAbbrev": "Feb", "daysInMonth": 28, "monthAbbrevEs": "Feb"},
    {"monthAbbrev": "Mar", "daysInMonth": 31, "monthAbbrevEs": "Mar"},
    {"monthAbbrev": "Apr", "daysInMonth": 30, "monthAbbrevEs": "Abr"},
    {"monthAbbrev": "May", "daysInMonth": 31, "monthAbbrevEs": "May"},
    {"monthAbbrev": "Jun", "daysInMonth": 30, "monthAbbrevEs": "Jun"},
    {"monthAbbrev": "Jul", "daysInMonth": 31, "monthAbbrevEs": "Jul"},
    {"monthAbbrev": "Aug", "daysInMonth": 31, "monthAbbrevEs": "Ago"},
    {"monthAbbrev": "Sep", "daysInMonth": 30, "monthAbbrevEs": "Sep"},
    {"monthAbbrev": "Oct", "daysInMonth": 31, "monthAbbrevEs": "Oct"},
    {"monthAbbrev": "Nov", "daysInMonth": 30, "monthAbbrevEs": "Nov"},
    {"monthAbbrev": "Dec", "daysInMonth": 31, "monthAbbrevEs": "Dic"}
  ]
},
    {
      "name": "dateWindowSize",
      "source": "allDates",
      "transform": [
        {
          "type": "formula",
          "expr": "(dateGranularity === 'day' ? datum.dateDR * dateStepWidth : dateGranularity === 'month' && utcFormat(datum.date, '%d') === '01' ? datum.monthDR * dateStepWidth : dateGranularity === 'year' &&  utcFormat(datum.date, '%m-%d') === '01-01' ? datum.yearDR*dateStepWidth : 0)",
          "as": "runningWidth"
        },
        {"type": "filter", "expr": "datum.runningWidth >0"},
        {
          "type": "window",
          "ops": ["row_number"],
          "sort": {"field": "date"},
          "as": ["rowNumber"]
        },
        {"type": "joinaggregate", "ops": ["count"], "as": ["rowCount"]},
        {
          "type": "formula",
          "expr": "extent(pluck(data('allDates'), 'date'))[0] === datum.date ? datum.rowNumber : 0",
          "as": "actualStartRowNumber"
        },
        {
          "type": "formula",
          "expr": "extent(pluck(data('allDates'), 'date'))[1] === datum.date ? datum.rowNumber : datum.rowCount",
          "as": "actualEndRowNumber"
        },
        {
          "type": "joinaggregate",
          "ops": ["max", "max"],
          "fields": ["actualStartRowNumber", "actualEndRowNumber"],
          "as": ["actualStartRowNumber", "actualEndRowNumber"]
        },
        {
          "type": "filter",
          "expr": "datum.runningWidth<=(ganttRange[1]-ganttRange[0])"
        },
        {
          "type": "aggregate",
          "ops": ["count"],
          "groupby": ["rowCount", "actualStartRowNumber", "actualEndRowNumber"],
          "as": ["windowRowCount", "actualStartRowNumber", "actualEndRowNumber"]
        },
        {
          "type": "formula",
          "expr": "datum.rowCount > datum.windowRowCount",
          "as": "scrollEnabled"
        }
      ]
    },
    {
      "name": "date",
      "source": "allDates",
      "transform": [
        {
          "type": "formula",
          "expr": "(dateGranularity === 'day' ? datum.dateDR * dateStepWidth : dateGranularity === 'month' && utcFormat(datum.date, '%d') === '01' ? datum.monthDR * dateStepWidth : dateGranularity === 'year' &&  utcFormat(datum.date, '%m-%d') === '01-01' ? datum.yearDR*dateStepWidth : 0)",
          "as": "runningWidth"
        },
        {"type": "filter", "expr": "datum.runningWidth >0"},
        {
          "type": "window",
          "ops": ["row_number"],
          "sort": {"field": "date"},
          "as": ["rowNumber"]
        },
        {"type": "joinaggregate", "ops": ["count"], "as": ["rowCount"]},
        {
          "type": "filter",
          "expr": "(datum.rowNumber >= (scale('minDateRow', horizontalScrollPercent)) && datum.rowNumber <= (scale('maxDateRow', horizontalScrollPercent))) || (dateGranularity === 'year'>=0)"
        }
      ]
    },
    {
      "name": "hierarchy_initial",
      "source": "dataset_formatted",
      "transform": [
        {"type": "formula", "expr": "resetLevel", "as": "resetLevel"},
        {
          "type": "filter",
          "expr": "configIncludeRoot ? true : (isValid(datum['parentId']))"
        },
        {
          "type": "formula",
          "expr": "length(treeAncestors('dataset_formatted', datum['id']))-(configIncludeRoot ? 0 : 1)",
          "as": "level"
        },
        {
          "type": "formula",
          "expr": "indexof(pluck(data('dataset_formatted'), 'parentId'), datum['id'])>=0",
          "as": "hasChildren"
        },
        {
          "type": "formula",
          "expr": "datum.hasChildren ? configRow.levelIndentWidth*(datum['level']-0.25) : null",
          "as": "expandCollapseIndicatorX"
        },
        {
          "type": "formula",
          "expr": "configRow.levelIndentWidth*datum['level']",
          "as": "indentWidth"
        },
        {
          "type": "formula",
          "expr": "slice(pluck(treeAncestors('dataset_formatted', datum['id']), 'id'), 1)",
          "as": "ancestorIds"
        },
        {
          "type": "formula",
          "expr": "pluck(data('dataset_formatted'), 'idObj')",
          "as": "immediateChildrenIds"
        },
        {
          "type": "flatten",
          "fields": ["immediateChildrenIds"],
          "as": ["immediateChildrenIds"]
        },
        {
          "type": "filter",
          "expr": "datum.id === datum.immediateChildrenIds.id || datum.id === datum.immediateChildrenIds.parentId"
        },
        {
          "type": "aggregate",
          "ops": ["values"],
          "fields": ["idObs"],
          "groupby": [
            "id",
            "parentId",
            "name",
            "startDate",
            "endDate",
            "decimalPercentComplete",
            "dependencyId",
            "sort",
            "level",
            "expandCollapseIndicatorX",
            "indentWidth",
            "ancestorIds",
            "hasChildren",
            "resetLevel",
            "color"
          ],
          "as": ["immediateChildrenIds"]
        },
        {
          "type": "formula",
          "expr": "pluck(slice(pluck(datum.immediateChildrenIds, 'immediateChildrenIds'), 1), 'id')",
          "as": "immediateChildrenIds"
        },
        {
          "type": "formula",
          "expr": "isValid(datum.isExpanded) ? datum.isExpanded : !datum.hasChildren ? null : datum.resetLevel > datum.level ? 1 : 0",
          "as": "isExpanded"
        },
        {
          "type": "formula",
          "expr": "!expandAllClicked && datum.hasChildren && isValid(lastClickedNode) && lastClickedNode.datum.id === datum.id ? lastClickedNode.datum.isExpanded === 1 ? 0 : 1 : datum.isExpanded",
          "as": "isExpanded"
        },
        {
          "type": "formula",
          "expr": "datum.hasChildren && isInitial ? datum.level >= datum.resetLevel ? 0 : 1 : datum.isExpanded",
          "as": "isExpanded"
        }
      ]
    },
    {
      "name": "hierarchy_hidden_ancestorIds",
      "source": "hierarchy_initial",
      "transform": [
        {"type": "filter", "expr": "datum.isExpanded === 0"},
        {"type": "filter", "expr": "datum.hasChildren"},
        {"type": "project", "fields": ["id", "name"]}
      ]
    },
    {
      "name": "hierarchy_master",
      "source": "hierarchy_initial",
      "transform": [
        {
          "type": "formula",
          "expr": "pluck(data('hierarchy_hidden_ancestorIds'), 'id')",
          "as": "collapsedIds"
        },
        {
          "type": "filter",
          "expr": "!test('(\\\\b\\\\d+\\\\b).*\\\\b\\\\1\\\\b', (join(datum.ancestorIds, ',')+','+join(datum.collapsedIds, ',')))"
        },
        {
          "type": "formula",
          "expr": "isInitial ? (datum.level <= datum.resetLevel) ? 1 : 0 : indexof(lastClickedNode.datum.immediateChildrenIds, datum.id)>=0 ? lastClickedNode.datum.isExpanded === 1 ? 1 : 0 : 1",
          "as": "isVisible"
        },
        {
          "type": "collect",
          "sort": {
            "field": ["isVisible", "sort"],
            "order": ["descending", "ascending"]
          }
        },
        {
          "type": "window",
          "ops": ["row_number"],
          "as": ["rowNumber"],
          "sort": {
            "field": ["isVisible", "sort"],
            "order": ["descending", "ascending"]
          }
        },
        {"type": "formula", "expr": "datum.rowNumber-1", "as": "rowNumber"},
        {
          "type": "formula",
          "expr": "!datum.hasChildren ? 0 : datum.isExpanded === 1 ? 90 : 0",
          "as": "expandCollapseIndicatorAngle"
        },
        {
          "type": "joinaggregate",
          "fields": ["sort"],
          "ops": ["max"],
          "as": ["isLastRow"]
        },
        {
          "type": "formula",
          "expr": "datum.sort === datum.isLastRow ? true : false",
          "as": "isLastRow"
        }
      ]
    },
    {
      "name": "hierarchy_master_gantt_area",
      "source": "hierarchy_master",
      "transform": [
        {"type": "collect", "sort": {"field": "sort"}},
        {
          "type": "filter",
          "expr": "datum.startDate <  (timeOffset('month', ganttDomain[1], (dateGranularity === 'month' ? 1 : 0))) && datum.endDate > ganttDomain[0]"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.startDate, '%b')",
          "as": "startAbbrevEs"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.endDate, '%b')",
          "as": "endAbbrevEs"
        },
        {
  "type": "lookup",
  "from": "monthLookup",
  "key": "monthAbbrev",
  "fields": ["startAbbrevEs"],
  "values": ["monthAbbrevEs"],
  "as": ["startAbbrevEs"]
},
{
  "type": "lookup",
  "from": "monthLookup",
  "key": "monthAbbrev",
  "fields": ["endAbbrevEs"],
  "values": ["monthAbbrevEs"],
  "as": ["endAbbrevEs"]
},
        {
          "type": "formula",
          "expr": "(datum.startAbbrevEs === 'Feb' && +utcFormat(datum.startDate, '%Y')%4 === 0 ? 1 : 0) + datum.daysInStartDateMonth",
          "as": "daysInStartDateMonth"
        },
        {
          "type": "formula",
          "expr": "(datum.endAbbrevEs === 'Feb' && +utcFormat(datum.endDate, '%Y')%4 === 0 ? 1 : 0) + datum.daysInEndDateMonth",
          "as": "daysInEndDateMonth"
        },
        {
          "type": "formula",
          "expr": "+utcFormat(datum.startDate, '%d')/datum.daysInStartDateMonth",
          "as": "startDatePercentageOfMonth"
        },
        {
          "type": "formula",
          "expr": "+utcFormat(datum.endDate, '%d')/datum.daysInEndDateMonth",
          "as": "endDatePercentageOfMonth"
        },
        {
          "type": "formula",
          "expr": "toDate(utcFormat(datum.startDate+((datum.endDate-datum.startDate)*datum.decimalPercentComplete), '%Y-%m-%d'))",
          "as": "progressDate"
        },
        {
          "type": "formula",
          "expr": "+utcFormat(datum.progressDate, '%d')/datum.daysInEndDateMonth",
          "as": "progressDatePercentageOfMonth"
        },
        {
          "type": "formula",
          "expr": "utcdayofyear(datum.startDate)/((utcFormat(datum.startDate, '%Y') % 4 === 0 && utcFormat(datum.startDate, '%Y') % 100 > 0) || utcFormat(datum.startDate, '%Y') %400 == 0 ? 366 : 365)",
          "as": "startDatePercentageOfYear"
        },
        {
          "type": "formula",
          "expr": "utcdayofyear(datum.endDate)/((utcFormat(datum.endDate, '%Y') % 4 === 0 && utcFormat(datum.endDate, '%Y') % 100 > 0) || utcFormat(datum.endDate, '%Y') %400 == 0 ? 366 : 365)",
          "as": "endDatePercentageOfYear"
        },
        {
          "type": "formula",
          "expr": "utcdayofyear(datum.progressDate)/((utcFormat(datum.progressDate, '%Y') % 4 === 0 && utcFormat(datum.progressDate, '%Y') % 100 > 0) || utcFormat(datum.progressDate, '%Y') %400 == 0 ? 366 : 365)",
          "as": "progressDatePercentageOfYear"
        },
        {"type": "formula", "expr": "dateGranularity", "as": "dateGranularity"},
        {
  "type": "formula",
  "expr": "indexof(['day', 'month', 'year'], datum.dateGranularity) > -1 ? [0,datum.startDatePercentageOfMonth, datum.startDatePercentageOfYear][indexof(['day', 'month', 'year'], datum.dateGranularity)] : 0",
  "as": "startDatePercentage"
},
        {
  "type": "formula",
  "expr": "indexof(['day', 'month', 'year'], datum.dateGranularity) > -1 ? [0,datum.startDatePercentageOfMonth, datum.startDatePercentageOfYear][indexof(['day', 'month', 'year'], datum.dateGranularity)] : 0",
  "as": "endDatePercentage"
},
        {
  "type": "formula",
  "expr": "indexof(['day', 'month', 'year'], datum.dateGranularity) > -1 ? [0,datum.startDatePercentageOfMonth, datum.startDatePercentageOfYear][indexof(['day', 'month', 'year'], datum.dateGranularity)] : 0",
  "as": "progressDatePercentage"
},
        /*
        {
          "type": "formula",
          "expr": "toDate([utcFormat(datum.startDate, '%m/%d/%Y'), utcFormat(datum.startDate, '%m')+'/01/'+utcFormat(datum.startDate, '%Y'), '01/01/'+utcFormat(datum.startDate, '%Y')][indexof(['day', 'month', 'year'], datum.dateGranularity)]+' 00:00:00.000')",
          "as": "startDateTick"
        },
        */
         {
          "type": "formula",
          "expr": "toDate([utcFormat(datum.startDate, '%Y-%m-%d'), utcFormat(datum.startDate, '%Y-%m-01'), utcFormat(datum.startDate, '%Y-01-01')][indexof(['day', 'month', 'year'], datum.dateGranularity)])",
          "as": "startDateTick"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.startDateTick, '%Y-%m-%d')",
          "as": "startDateKey"
        },
        {
          "type": "lookup",
          "key": "dateKey",
          "from": "allDates",
          "fields": ["startDateKey"],
          "values": ["date"],
          "as": ["startDateTick"]
        },
        /*{
          "type": "formula",
          "expr": "toDate([utcFormat(datum.endDate, '%Y-%m-%d'), utcFormat(datum.endDate, '%m')+'/01/'+utcFormat(datum.endDate, '%Y'), '01/01/'+utcFormat(datum.endDate, '%Y')][indexof(['day', 'month', 'year'], datum.dateGranularity)])",
          "as": "endDateTick"
        },*/
        {
          "type": "formula",
          "expr": "toDate([utcFormat(datum.endDate, '%Y-%m-%d'), utcFormat(datum.endDate, '%Y-%m-01'), utcFormat(datum.endDate, '%Y-01-01')][indexof(['day', 'month', 'year'], datum.dateGranularity)])",
          "as": "endDateTick"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.endDateTick, '%Y-%m-%d')",
          "as": "endDateKey"
        },
        {
          "type": "lookup",
          "key": "dateKey",
          "from": "allDates",
          "fields": ["endDateKey"],
          "values": ["date"],
          "as": ["endDateTick"]
        },
        /*{
          "type": "formula",
          "expr": "toDate([utcFormat(datum.progressDate, '%m/%d/%Y'), utcFormat(datum.progressDate, '%m')+'/01/'+utcFormat(datum.progressDate, '%Y'), '01/01/'+utcFormat(datum.progressDate, '%Y')][indexof(['day', 'month', 'year'], datum.dateGranularity)]+' 00:00:00.000')",
          "as": "progressDateTick"
        },*/
        {
          "type": "formula",
          "expr": "toDate([utcFormat(datum.progressDate, '%Y-%m-%d'), utcFormat(datum.progressDate, '%Y-%m-01'), utcFormat(datum.progressDate, '%Y-01-01')][indexof(['day', 'month', 'year'], datum.dateGranularity)])",
          "as": "progressDateTick"
        },
        {
          "type": "formula",
          "expr": "datum.decimalPercentComplete === 0 ? datum.startDateTick : datum.decimalPercentComplete === 1 ? datum.endDateTick : datum.progressDateTick",
          "as": "progressDateTick"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.progressDateTick, '%Y-%m-%d')",
          "as": "progressDateKey"
        },
        {
          "type": "lookup",
          "key": "dateKey",
          "from": "allDates",
          "fields": ["progressDateKey"],
          "values": ["date"],
          "as": ["progressDateTick"]
        },
        {
          "type": "formula",
          "expr": "(scale('xBandScaleDate', datum.startDateTick)+bandwidth('xBandScaleDate')*datum.startDatePercentage) || ganttRange[0]",
          "as": "x1"
        },
        {
          "type": "formula",
          "expr": "(scale('xBandScaleDate', datum.endDateTick)+bandwidth('xBandScaleDate')*datum.endDatePercentage) || ganttRange[1]",
          "as": "x2"
        },
        {
          "type": "formula",
          "expr": "(scale('xBandScaleDate', datum.progressDateTick)+bandwidth('xBandScaleDate')*datum.progressDatePercentage) || ganttRange[1]",
          "as": "x2Progress"
        },
        {
          "type": "formula",
          "expr": "datum.startDate === datum.endDate ? 'milestone' : datum.isExpanded ? 'parentRect':'childRect'",
          "as": "shapeType"
        },
        {
  "type": "formula",
  "expr": "isValid(datum.name) && isValid(datum.decimalPercentComplete) ? datum.name + ' (' + format(datum.decimalPercentComplete, '.0%') + ')' : ''",
  "as": "label"
},
       {
          "type": "project",
          "fields": [
            "id",
            "rowNumber",
            "name",
            "startDate",
            "endDate",
            "startDateKey",
            "endDateKey",
            "progressDate",
            "decimalPercentComplete",
            "level",
            "x1",
            "x2",
            "x2Progress",
            "color",
            "shapeType",
            "label"
          ]
        },
        {"type": "collect", "sort": {"field": "rowNumber"}}
      ]
    },
    {
      "name": "today",
      "values": "[{}]",
      "transform": [
        {
          "type": "formula",
          "expr": "toDate(utcFormat(now(), '%m/%d/%Y'))"
,
          "as": "date"
        },
        {
          "type": "formula",
          "expr": "+utcFormat(timeOffset('day', timeOffset('month', toDate(utcFormat(datum.date, '%m')+'/01/'+utcFormat(datum.date, '%Y')), 1),-1), '%d')",
          "as": "daysInMonth"
        },
        {
          "type": "formula",
          "expr": "+utcFormat(datum.date, '%d')/datum.daysInMonth",
          "as": "datePercentageOfMonth"
        },
        {
          "type": "formula",
          "expr": "utcdayofyear(datum.date)/((utcFormat(datum.date, '%Y') % 4 === 0 && utcFormat(datum.date, '%Y') % 100 > 0) || utcFormat(datum.date, '%Y') %400 == 0 ? 366 : 365)",
          "as": "datePercentageOfYear"
        },
        {"type": "formula", "expr": "dateGranularity", "as": "dateGranularity"},
        {
          "type": "formula",
          "expr": "[0.5,datum.datePercentageOfMonth, datum.datePercentageOfYear][indexof(['day', 'month', 'year'], datum.dateGranularity)]",
          "as": "datePercentage"
        },
        {
          "type": "formula",
          "expr": "toDate([utcFormat(datum.date, '%m/%d/%Y'), utcFormat(datum.date, '%m')+'/01/'+utcFormat(datum.date, '%Y'), '01/01/'+utcFormat(datum.date, '%Y')][indexof(['day', 'month', 'year'], datum.dateGranularity)]+' 00:00:00.000')",
          "as": "dateTick"
        },
        {
          "type": "formula",
          "expr": "utcFormat(datum.dateTick, '%Y-%m-%d')",
          "as": "dateKey"
        },
        {
          "type": "lookup",
          "key": "dateKey",
          "from": "allDates",
          "fields": ["dateKey"],
          "values": ["date"],
          "as": ["dateTick"]
        },
        {
          "type": "formula",
          "expr": "(scale('xBandScaleDate', toDate(datum.dateTick))+bandwidth('xBandScaleDate')*datum.datePercentage)",
          "as": "x"
        },
        {
          "type": "formula",
          "expr": "firstVisibleRowId",
          "as": "firstVisibleRowId"
        },
        {
          "type": "formula",
          "expr": "extent([secondVisibleRowId, 2])[1]",
          "as": "secondVisibleRowId"
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master_gantt_area",
          "fields": ["firstVisibleRowId"],
          "values": ["endDateKey"],
          "as": ["endDateKey1"]
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master_gantt_area",
          "fields": ["secondVisibleRowId"],
          "values": ["endDateKey"],
          "as": ["endDateKey2"]
        },
        {
          "type": "formula",
          "expr": "indexof([datum.endDateKey1, datum.endDateKey2], datum.dateKey) >= 0 ? 0 : 1",
          "as": "labelOpacity"
        }
      ]
    },
    {
      "name": "dependencyLinks",
      "source": "dataset_formatted",
      "transform": [
        {"type": "filter", "expr": "isValid(datum.dependencyId)"},
        {
          "type": "formula",
          "expr": "split(datum.dependencyId, ',')",
          "as": "dependencyId"
        },
        {"type": "flatten", "fields": ["dependencyId"]},
        {
          "type": "project",
          "fields": ["dependencyId", "id"],
          "as": ["sourceId", "targetId"]
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master",
          "values": ["endDate", "rowNumber"],
          "fields": ["sourceId"],
          "as": ["sourceEndDate", "sourceRowNumber"]
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master",
          "values": ["startDate", "rowNumber"],
          "fields": ["targetId"],
          "as": ["targetStartDate", "targetRowNumber"]
        },
        {
          "type": "formula",
          "expr": "inrange(datum.sourceEndDate, domain('xBandScaleDate'))",
          "as": "sourceXInRange"
        },
        {
          "type": "formula",
          "expr": "inrange(datum.targetStartDate, domain('xBandScaleDate'))",
          "as": "targetXInRange"
        },
        {
          "type": "formula",
          "expr": "datum.sourceRowNumber*configRow.rowHeight+configRow.rowHeight/2",
          "as": "sourceY"
        },
        {
          "type": "formula",
          "expr": "datum.targetRowNumber*configRow.rowHeight+configRow.rowHeight/2",
          "as": "targetY"
        },
        {
          "type": "filter",
          "expr": "datum.sourceXInRange || datum.targetXInRange"
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master_gantt_area",
          "values": ["x2"],
          "fields": ["sourceId"],
          "as": ["sourceX"]
        },
        {
          "type": "lookup",
          "key": "id",
          "from": "hierarchy_master_gantt_area",
          "values": ["x1"],
          "fields": ["targetId"],
          "as": ["targetX"]
        },
        {
          "type": "formula",
          "expr": "datum.sourceX || ganttRange[0]",
          "as": "sourceX"
        },
        {
          "type": "formula",
          "expr": "datum.targetX || ganttRange[1]",
          "as": "targetX"
        },
        {
          "type": "formula",
          "expr": "{x: datum.sourceX, y: datum.sourceY}",
          "as": "source"
        },
        {
          "type": "formula",
          "expr": "{x: datum.targetX, y: datum.targetY}",
          "as": "target"
        },
        {"type": "fold", "fields": ["source", "target"], "as": ["key", "xy"]},
        {
          "type": "formula",
          "expr": "datum.sourceId+'-'+datum.targetId",
          "as": "sourceTargetCompId"
        },
        {
          "type": "window",
          "ops": ["dense_rank"],
          "sort": {"field": "sourceTargetCompId"},
          "as": ["linkId"]
        },
        {
          "type": "formula",
          "expr": "(datum.targetY-datum.sourceY)/2",
          "as": "halfHeight"
        },
        {
          "type": "formula",
          "expr": "datum.targetRowNumber-datum.sourceRowNumber",
          "as": "rowSeparationCount"
        },
        {
          "type": "project",
          "fields": [
            "linkId",
            "sourceId",
            "targetId",
            "sourceXInRange",
            "targetXInRange",
            "halfHeight",
            "rowSeparationCount",
            "key",
            "xy"
          ]
        },
        {"type": "formula", "expr": "[1,2,3]", "as": "sort"},
        {"type": "flatten", "fields": ["sort"]},
        {
          "type": "formula",
          "expr": "datum.sort+(datum.key === 'source' ? 0 : 3)",
          "as": "sort"
        },
        {
          "type": "formula",
          "expr": "datum.sort === 2 ? {x: datum.xy.x+15, y:datum.xy.y} : datum.xy",
          "as": "xy"
        },
        {
          "type": "formula",
          "expr": "datum.sort === 3 ? {x:datum.xy.x+15, y:datum.xy.y+datum.halfHeight} : datum.xy",
          "as": "xy"
        },
        {
          "type": "formula",
          "expr": "datum.sort === 4 ? {x:datum.xy.x-15, y:datum.xy.y-datum.halfHeight} : datum.xy",
          "as": "xy"
        },
        {
          "type": "formula",
          "expr": "datum.sort === 5 ? {x:datum.xy.x-15, y:datum.xy.y} : datum.xy",
          "as": "xy"
        },
        {
          "type": "formula",
          "expr": "inrange(datum.xy.x, [ganttRange[0], ganttRange[1]]) ? 1 : 0",
          "as": "opacity"
        },
        {
          "type": "window",
          "ops": ["min"],
          "fields": ["opacity"],
          "groupby": ["linkId"],
          "frame": [null, null],
          "as": ["opacity"]
        }
      ]
    },
    {
      "name": "currentMaxIndentWidth",
      "source": "hierarchy_initial",
      "transform": [
        {
          "type": "aggregate",
          "ops": ["max"],
          "fields": ["level"],
          "as": ["currentMaxLevel"]
        },
        {
          "type": "formula",
          "expr": "(datum.currentMaxLevel-1)*configRow.levelIndentWidth",
          "as": "indent"
        }
      ]
    },
    {
      "name": "height",
      "source": "hierarchy_master",
      "transform": [
        {"type": "filter", "expr": "datum.isVisible === 1"},
        {"type": "aggregate", "ops": ["count"], "as": ["height"]},
        {
          "type": "formula",
          "expr": "(datum.height)*configRow.rowHeight",
          "as": "height"
        }
      ]
    },
    {
      "name": "LeftHandColumns",
      "values": [{}],
      "transform": [
        {
          "type": "formula",
          "expr": "configIncludedLeftHandColumns",
          "as": "column"
        },
        {"type": "flatten", "fields": ["column"]},
        {
          "type": "formula",
          "expr": "currentMaxIndentWidth",
          "as": "currentMaxIndentWidth"
        },
        {
          "type": "filter",
          "expr": "indexof(['name','startDate','endDate','duration','progress'], datum.column) >= 0"
        },
        {
          "type": "filter",
          "expr": "showDetails ? true : datum.column==='name'"
        },
        {
          "type": "formula",
          "expr": "['Proyecto', 'Inicio', 'Fin', 'Días', '%'][indexof(['name','startDate','endDate','duration','progress'], datum.column)]",
          "as": "label"
        },
        {
          "type": "formula",
          "expr": "indexof(['name','startDate','endDate','duration','progress'], datum.column)",
          "as": "rowNumber"
        },
        {
          "type": "window",
          "ops": ["row_number"],
          "sort": {"field": "rowNumber"},
          "as": ["rowNumber"]
        },
        {"type": "collect", "sort": {"field": "rowNumber"}},
        {
          "type": "formula",
          "expr": "[configColumn.columns.taskTitle.align, configColumn.columns.startDate.align, configColumn.columns.endDate.align, configColumn.columns.duration.align, configColumn.columns.progress.align][indexof(['name','startDate','endDate','duration','progress'], datum.column)]",
          "as": "align"
        },
        {
          "type": "formula",
          "expr": "[configColumn.columns.taskTitle.allowableWidth, configColumn.columns.startDate.allowableWidth, configColumn.columns.endDate.allowableWidth, configColumn.columns.duration.allowableWidth, configColumn.columns.progress.allowableWidth][indexof(['name','startDate','endDate','duration','progress'], datum.column)]",
          "as": "allowableWidth"
        },
        {
          "type": "window",
          "ops": ["sum"],
          "fields": ["allowableWidth"],
          "sort": {"field": "rowNumber", "order": "ascending"},
          "frame": [null, 0],
          "as": ["x"]
        },
        {"type": "formula", "expr": "datum.x+configColumn.xOffset", "as": "x"},
        {
          "type": "formula",
          "expr": "datum.x+(datum.align === 'right' ? datum.allowableWidth : 0)",
          "as": "x"
        },
        {
          "type": "formula",
          "expr": "(datum.x-datum.allowableWidth)+(configColumn.innerPadding*(datum.rowNumber-1))+(datum.column==='name' ? 0 : datum.currentMaxIndentWidth)",
          "as": "x"
        },
        {
          "type": "window",
          "ops": ["last_value", "last_value"],
          "fields": ["allowableWidth", "x"],
          "sort": {"field": "rowNumber"},
          "frame": [null, null],
          "as": ["lastAllowableWidth", "lastX"]
        },
        {
          "type": "formula",
          "expr": "(datum.lastAllowableWidth+datum.lastX)*(showDetails ? 1 : 0.95)",
          "as": "totalWidth"
        }
      ]
    },
    {
      "name": "expandAllCollapseAllConfig",
      "values": [
        {
          "rowNumber": 0,
          "name": "expandAll",
          "path": "M0 96C0 78.3 14.3 64 32 64H416c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 128 0 113.7 0 96zM64 256c0-17.7 14.3-32 32-32H480c17.7 0 32 14.3 32 32s-14.3 32-32 32H96c-17.7 0-32-14.3-32-32zM448 416c0 17.7-14.3 32-32 32H32c-17.7 0-32-14.3-32-32s14.3-32 32-32H416c17.7 0 32 14.3 32 32z",
          "size": 0.00475,
          "tooltip": "Expandir Todo"
        },
        {
          "rowNumber": 1,
          "name": "collapseAll",
          "path": "M0 96C0 78.3 14.3 64 32 64H416c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 128 0 113.7 0 96zM0 256c0-17.7 14.3-32 32-32H416c17.7 0 32 14.3 32 32s-14.3 32-32 32H32c-17.7 0-32-14.3-32-32zM448 416c0 17.7-14.3 32-32 32H32c-17.7 0-32-14.3-32-32s14.3-32 32-32H416c17.7 0 32 14.3 32 32z",
          "size": 0.00475,
          "fontWeight": 400,
          "tooltip": "Colapsar Todo"
        }
      ]
    },
    {
      "name": "lastDateGranularity",
      "values": [],
      "on": [
        {
          "trigger": "dateStepSizePercent",
          "insert": "{lastDateGranularity: dateGranularity, time: now()}"
        }
      ],
      "transform": [
        {
          "type": "window",
          "ops": ["row_number"],
          "sort": {"field": "time", "order": "descending"},
          "as": ["rowNumber"]
        },
        {"type": "filter", "expr": "datum.rowNumber===2"}
      ]
    }
  ]
}
