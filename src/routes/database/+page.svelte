<script>
  async function list() {
    const query = `
      query Pomodoros {
        pomodoros {
            items {
                id
                category
                user
                duration
            }
        }
      }`;

    const endpoint = '/data-api/graphql';
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ query: query })
    });
    const result = await response.json();
    console.table(result.data.pomodoros.items);
  }

  async function get() {
    const id = '2';

    const gql = `
      query Pomodoro_by_pk($id: ID!) {
        pomodoro_by_pk(id: $id) {
            id
            category
            user
            duration
        }
      }`;

    const query = {
      query: gql,
      variables: {
        id: id
      }
    };

    const endpoint = '/data-api/graphql';
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(query)
    });
    const result = await response.json();
    console.table(result.data.pomodoro_by_pk);
  }

  async function update() {
    const id = '4';
    const data = {
      id: id,
      category: 'break',
      user: 'Jane Doe',
      duration: 10
    };

    const gql = `
      mutation UpdatePomodoro($id: ID!, $_partitionKeyValue: String!, $item: UpdatePomodoroInput!) {
        updatePomodoro(id: $id, _partitionKeyValue: $_partitionKeyValue, item: $item) {
            id
            category
            user
            duration
        }
      }`;

    const query = {
      query: gql,
      variables: {
        id: id,
        _partitionKeyValue: id,
        item: data
      }
    };

    const endpoint = '/data-api/graphql';
    const res = await fetch(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(query)
    });

    const result = await res.json();
    console.table(result.data.updatePomodoro);
  }

  async function create() {
    const data = {
      id: '6',
      category: 'work',
      user: 'Bob',
      duration: 20
    };

    const gql = `
    mutation CreatePomodoro($item: CreatePomodoroInput!) {
      createPomodoro(item: $item) {
          id
          category
          user
          duration
      }
    }`;

    const query = {
      query: gql,
      variables: {
        item: data
      }
    };

    const endpoint = '/data-api/graphql';
    const result = await fetch(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(query)
    });

    const response = await result.json();
    console.table(response.data.createPomodoro);
  }

  async function del() {
    const id = '6';

    const gql = `
    mutation DeletePomodoro($id: ID!, $_partitionKeyValue: String!) {
      deletePomodoro(id: $id, _partitionKeyValue: $_partitionKeyValue) {
          id
          category
          user
          duration
      }
    }`;

    const query = {
      query: gql,
      variables: {
        id: id,
        _partitionKeyValue: id
      }
    };

    const endpoint = '/data-api/graphql';
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(query)
    });

    const result = await response.json();
    console.log(`Record deleted: ${JSON.stringify(result.data)}`);
  }
</script>

<h1>Static Web Apps with Database Connections</h1>
<blockquote>Open the console in the browser developer tools to see the API responses.</blockquote>
<div>
  <button id="list" on:click={list}>List</button>
  <button id="get" on:click={get}>Get</button>
  <button id="update" on:click={update}>Update</button>
  <button id="create" on:click={create}>Create</button>
  <button id="delete" on:click={del}>Delete</button>
</div>
