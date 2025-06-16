[**@obolnetwork/obol-sdk**](../index.md)

***

[@obolnetwork/obol-sdk](../index.md) / HttpRequestFunc

> **HttpRequestFunc** = (`url`, `config`?) => `Promise`\<`any`\>

Defined in: [types.ts:507](https://github.com/ObolNetwork/obol-sdk/blob/d77f4594233f658ddb52882926187420144e316d/src/types.ts#L507)

Generic HTTP request function type.
Args:
 url: string - The URL to request.
 config?: Record<string, any> - Optional request configuration (e.g., method, headers, body for POST).
Returns:
 Promise<any> - The response data.

## Parameters

| Parameter | Type |
| ------ | ------ |
| `url` | `string` |
| `config`? | `Record`\<`string`, `any`\> |

## Returns

`Promise`\<`any`\>
