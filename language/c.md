# C

TODO: review files for other ntoes

## Heap Allocations

- Ensure all allocated memory is deallocated. After deallocation, set the pointer to `NULL`.
- If `realloc` fails, set the passed pointer to `NULL`.
- Before freeing sensitive data, consider manually reinitializing that memory to zero.

