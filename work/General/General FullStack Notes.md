### 1. Tanstack query
- The query key holds the name of cache that tanstack query creates, and any where you create a fetcher that has the same query key, it will check the cache first which means you can create multiple hooks that check the same shared cache
- Keywords like list or detail in query keys are purely conventional, and have no real meaning to tanstack itself,
	- [users] != [users, list] != [list, users]
	- tanstack query key names get stringified when looking for the cache, so order and type matters
- However, the conventions are still helpful, as tanstack has hierarchical invalidation
	- `queryClient.invalidateQueries({ queryKey: ['users'] })` invalidates every key that has the word "users"
### 2. Tanstack router
- You can create mutation functions with {mutate: funcName} = useMutate, which returns the result of mutate as you're destructuring it from useMutate. Or funName = useMutate, which has the entire mutation object, then you can say funcName.mutate to apply a mutation