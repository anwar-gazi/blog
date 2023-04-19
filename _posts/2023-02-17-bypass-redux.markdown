---
layout: post
title:  "Redux: how to quickly avoid using redux for a while, yet sticking to the idea"
date:   2023-02-17 11:00:00 +0600
img: post/react-hooks.jpg
categories: react, redux
comments: true
---

Redux is built with some neat concepts. Like state managers, state diff, moving database and network stuffs away from the actual code etc. But while in hurry, reading through the redux docs can be "daunting"; and in some sense it is, as I advocate the online docs to be more like information pile at best, haystack in general and clutter at worst. You only need to get a good hand at the concepts and the idea to application path.
Anyways, you can apply this get/suggest pattern without using any library.
It is simple.
To get data from backend/data storage, you call get function. Set send changes to the backend, you call suggest function.
You write these function at your convenience, but in some specific organizations to keep your code more readable.
The way you represent the backend data in browser has not the same structure as it is received from the backend. So you need a formatter function. This formatter takes backend data and returns state.
Now frontend user input or interaction resulted changes are considered as state-partials. To send these changes to backend, we use the suggest_part function that takes the state-partial. the suggest function takes the full state object, which we are not dealing at this time.
Inside the suggest_part function, you may need a data formatter to reformat the state-partial(which is frontend data) to a format(array or key-val pairs) better suited for the transport to the backend.
Finally, you put these functions, get, suggest_part, suggest in a module named data_exchange.js as these functions are more like data exchange utils or helpers.

See this organization with an example using mui-datagrid:
Two files here, datagrid.js, data_exchange.js

```
// datagrid.js

import {DataGrid} from "@mui/x-data-grid";
import {useEffect, useState} from "react";
import {IconButton} from "@mui/material";
import {Edit} from "@mui/icons-material";
import {get, suggest_part} from "./data_exchange";

const cols = [
    {field: "col1", headerName: "APIKEY"},
    {field: "col2", headerName: "Enabled", editable: true},
    {field: "col3", headerName: "Rate Limit", editable: true},
    {
        field: "edit", headerName: "Edit", renderCell: (params) => {
            return <IconButton><Edit/></IconButton>;
        }
    }
];

const ApikeyGrid = () => {
    const [rows, setRows] = useState([]); 
    useEffect(() => {
        get((data) => {
            setRows(data);
        });
    }, []);
    useEffect(() => {
        if (!rows.length) {
            console.log("render");
        } else {
            console.log("update");
        }
    });

    return <div style={{width: "100%", height: "500px"}}>
        <DataGrid editMode="row" rows={rows} columns={cols} onEditCellPropsChange={(editObject) => {
            suggest_part(editObject['id'], {[editObject['field']]: editObject['props']['value']}, (suggestion_success)=>{
                if (suggestion_success) {
                    alert("update applied");
                } else {
                    alert("failed!");
                }
            });
        }}></DataGrid>
    </div>;
};

export default ApikeyGrid;
```

```
// data_exchange.js
import {read, utils} from "xlsx";

const backend_data_formatter = (backend_data) => {
    return backend_data.map((backend_row, i) => {
        return {id: i, col1: backend_row['apikey'], col2: backend_row['enabled'], col3: backend_row['rate']};
    });
};

export default async function getData(url) {
    const request = new Request(url, {method: "GET",});
    const buffer = await (await fetch(request)).arrayBuffer();
    const wb = read(buffer);
    return utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]);
}

const get = (then) => {
    getData("http://localhost:3001/db").then((data) => {
        then(backend_data_formatter(data));
    });
};

const suggest_part = (row_index, col_data, then) => {
    const data = [row_index, col_data];
    const req = new Request("http://localhost:3001/db", {method: "POST", headers: {"Content-Type": "application/json"}, body: JSON.stringify(data)});
    fetch(req).then((resp)=>{
        
    });
};

export {get, suggest_part};
```