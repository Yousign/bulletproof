# 📡 API Layer

First of all, please note that the Yousign app API is a REST api specified using OAS in `/oas/AppApi`.

The communication with the API is split in 2 distincts layers:

- An auto-generated Typescript client using openAPI tools, exposing available endpoints and business entities as TS interfaces.  
  It's powered by Axios under the hood.
- The usage of its client in React using `react-query` library. If you are not familiar with the lib, please read the [documentation](https://react-query-v3.tanstack.com/).

## Typescript client

The generated Typescript API client lives in `@yousign/api`.  

There are 2 instances for the same API so far:
- An instance for the authenticated part of the application
- An instance for the signature part

## Define and export queries declarations

Instead of declaring `react-query` queries and mutations on the go, have them defined and exported into `/api` folder of each features.

### Example of a query delcaration:
```
import { useQuery, useQueryClient } from 'react-query';

import {
  GetDiscussionResponse,
  ApiError,
} from "@yousign/api";
import { useApiClient } from '@/api';

export const queryKey = (id: string) => ['discussion', id]

export const useFetchDiscussion = (id: string) => {
  const fetchDiscussion = async () =>{
    const { data } = await useApiClient.getDiscussion<
      GetDiscussionResponse,
      ApiError
    >(id);
    return data;
  }

  return useQuery<GetDiscussionResponse>(queryKey(id), fetchDiscussion);
};

export const useInvalidateDiscussion = (id: string) => {
  const queryClient = useQueryClient();

  return (id: string) => queryClient.invalidateQueries(queryKey(id));
};
```

💡 A query is almost always accompanied by an invalidate hook so we can refresh the query (i.e after a mutation).


### Example of a mutation delcaration:

```
  import { useMutation } from 'react-query';

  import { CreateDiscussion, ApiError } from "@yousign/api";
  import { useApiClient } from '@/api';

  export const useCreateDicussion = () => {
      const createDiscussion = (discussion: CreateDiscussion) => {
        return useApiClient.createDiscussion(discussion);
      };

    return useMutation<void, ApiError>(createDiscussion);
  };
```

💡 A mutation often need to perform something on success or error, like to invalidate the data of the view so the user can see the results of a mutation.

To do that, we strongly encourage you to do it contextually, using the second argument of the mutate function.

```
const DiscussionForm = () => {
  const { mutate } = useCreateDicussion();

  […]

  const handleSubmit = (formValues: FormValues) => {
    mutate(formValues, {
      onSucces: () => {
        alert("success");
      },
      onError: () => {
        alert('Something when wrong')
      },
    })
  }

  return <form onSubmit={handleSubmit} {…} />
}
````
