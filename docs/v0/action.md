# Default Params

Saat `action` kita hit via Qore-App, parameter yang dikirim balik itu seperti dibawah,
> args
```json
{"payload_0": "1"}
```
Payload yang dikirim semua berupa string, jangan lupa di konversi sesuai keperluan untuk selain string.

> global
```json
{
    "QORE_DATA_ADMIN_SECRET": "RANDOM_TOKEN",
    "QORE_DATA_ENDPOINT": "https://staging-qore-data-xxxxxx-xxxxxx.xxxxxx.id",
    "QORE_PIPELINE_ADMIN_SECRET": "RANDOM_PIPELINE_TOKEN",
    "QORE_PIPELINE_ENDPOINT": "https://staging-qore-pipeline-xxxxxx-xxxxxx.xxxxxx.id"
}
```
Endpoint bisa berubah **(staging/production)** sesuai environment yang sedang kita gunakan, tetapi secret-nya tetap sama.

> row
```json
{
    "id": 0,
    "name": null,
    "description": null
}
```
Normalnya tidak akan menyertakan kolom `many/view` kecuali spesifik disebutkan di kolom ini, tipenya nanti **object array**. Dan untuk yang `one/id-relation` kalau spesifik disebutkan akan menampilkan **object**, kalau tidak hanya `id` sesuai tipe yang didefinisikan.

![Alt text](image.png)

> user

```json
{
			"data": {
				"active": true,
				"auth_provider": null,
				"created_at": "2022-12-12T09:27:46.083961Z",
				"email": null,
				"email_verified_at": null,
				"external_id": "xxxxxx@feedloop.ai",
				"id": 25,
				"linked_users": null,
				"refresh_tokens": [],
				"role": "xxxxxx-xxxx-xxxx-xxxx-xxxxxx",
				"role_name": "Super Admin",
				"updated_at": "2023-12-04T07:48:32.23791Z",
			},
			"id": "xxxxxx@feedloop.ai",
			"role": "Super Admin",
			"tenant": {
				"admin_secret": "RANDOM_TOKEN",
				"global_env": {
					"QORE_DATA_ADMIN_SECRET": "RANDOM_TOKEN",
					"QORE_DATA_ENDPOINT": "https://staging-qore-data-xxxxxx-xxxxxx.xxxxxx.id",
					"QORE_PIPELINE_ADMIN_SECRET": "RANDOM_TOKEN",
					"QORE_PIPELINE_ENDPOINT": "https://staging-qore-pipeline-xxxxxx-xxxxxx.xxxxxx.id"
				},
				"host": "staging-qore-data-xxxxxx-xxxxxx.xxxxxx.id",
				"jwt_secret": "RANDOM_TOKEN",
				"namespace": "team-1-xxxxxx-org-1-xxxxxx-xxxxxx-qore-data-staging",
				"schema": "staging-qore-data-xxxxxx-xxxxxx",
				"stage": "staging"
			}
		}
```

# Define Axiod instance

Kalian bisa cari library lain di `https://deno.land/std@0.208.0`, baik standard library atau third-party kayak `Axiod`.
Bisa kita definisikan seperti ini sebelum `main()`,

```js
import axiod from 'https://deno.land/x/axiod/mod.ts'
```

Lalu kita gunakan seperti ini,
```js
await axiod({
    method: 'post',
    url: `https://example.xxx`,
    headers: {
        'Accept': '*/*',
        'Source': global.QORE_DATA_ENDPOINT,
    },
    data: {
      payload_0: 'hierarchy',
    }
  })
```
*Disarankan untuk mendefinisikan headers tidak terpisah untuk code-readibility yang lebih baik*
