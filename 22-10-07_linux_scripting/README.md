# Exercise 07/10/22

Exercise to learn the Basics for linux scripting, JSON and deployment

We will build a dashboard for proccesses running on ec2 instances

### 1. Monitoring script
We will write a script that outputs a json with running processes
1. Search for and read the baiscs about the JSON format and try to understand the following JSON file 
```
[
    {
        "pid":234,
        "cpu": 30,
        "user": "fabianschmauder"
    },
    {
        "pid":134,
        "cpu": 20,
        "user": "fabianschmauder"
    },
    {
        "pid":33,
        "cpu": 10,
        "user": "fabianschmauder"
    }
]
```
2. write a script that writes the 5 processes with the highest cpu consumption into a json file. The file should look like the file from point 1 with the name `monitoring-data.json` (you can use `ps -eo pcpu,pid,user,args | sort -k 1 -r` to list the running proccesses )

### 2. Monitoring ec2 instance
1. upload your script to an ec2 instance
1. create a cron job that runs you script every 5 minutes

### 3. Upload to s3
1. create a s3 bucket
1. upload the monitoring result from you ec2 instance to the s3 bucket (you have to select a role)

### 4. Optional: create react app
1. install node on your local machine
1. go to a folder where you want to create a new project
1. use the command `npx create-react-app my-app --template typescript` that you find [here](https://create-react-app.dev/docs/adding-typescript/) to create a new react app
1. start you react app by going into the created folder and type `npm start`

### 5. Optional: deploy react app
1. use the existing s3 bucket and enable website hosting
1. create a script that uses `npm run build` to build your frontend project
1. create a script that upload the content of the build directory to the s3 bucket

### 6. Optional: deploy pipeline
1. create a new github repository
1. push your created project to github
1. create a pipeline that uses your scripts 
    1. build your project (you will need to setup node in your pipeline see [here](https://github.com/actions/setup-node))
    1. upload the build folder to the s3 bucket


### 7. Optional: Init dashboard
1. Replace the file `src > App.tsx` with the following content
```
import React, { useEffect, useState } from 'react';

interface Ec2Data {
  pid: string
  cpu: number
  user: string
}

function App() {
  const [ec2Data, setEc2Data] = useState<Ec2Data[]>();
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(false);

  useEffect(() => {
    setLoading(true);
    setError(false);
    fetch("/monitoring-data.json")
      .then(response => response.json())
      .then(setEc2Data)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);

  return (
    <main >
      <h1>Fancy Dashboard</h1>
      <table>
        <thead>
          <tr>
            <th>ec2 instance</th>
            <th>cpu</th>
            <th>memory</th>
          </tr>
        </thead>
        <tbody>
          {ec2Data?.map(row => 
          <tr key={row?.pid || '-'}>
            <td>
              {row?.pid || '-'}
            </td>

            <td>
              {row?.cpu || '-'}%
            </td>

            <td>
              {row?.user || '-'}
            </td>
          </tr>
          )}
        </tbody>
      </table>
      {error && <p>failed to load data</p>}
      {loading &&
        //@ts-ignore
        <progress intermidate="true"></progress>}
    </main>
  );
}

export default App;
```
1. save an example `monitoring-data.json` json to the public folder
1. (Optional) Style your page by adding the link tag to the index.html > head tag 
```
<link
    rel="stylesheet"
    href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css"
  />
```
1. push your changes ( upload your page with github actions)
1. check that the page loads the ec2 data (make sure your ec2 script creates a valid public json with the name `monitoring-data.json`  at the root directory)
