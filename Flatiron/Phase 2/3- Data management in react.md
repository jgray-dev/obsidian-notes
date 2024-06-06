Using json-server, 
`json-server db.json --port 4000`
Must set the port explicitly otherwise react and json-server will overlap on port 3000

Always put fetch calls that update data inside a function or button or other event, to avoid infinite loops.
`Fetch loads data -> Data updates state -> state refreshes page -> Fetch loads data ....`

When initializing state, use the same type as the data you will load - arrays should default with `[]`, text should be `""`, 