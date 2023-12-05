# Execute

Default URL: `${global.QORE_DATA_ENDPOINT}/v1/execute`. Contoh,

```js
{
    operations: [{
        operation: 'Select',
        instruction: {
            table: 'TABLE_NAME',
            name: `PER_ACTION_NAME`,
        },
    }],
}
```

# Select

Bentuk paling sederhana dari `select` seperti dibawah,
```js
{
	operation: 'Select',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
	},
}
```
*Nama dari operation bersifat unik, jika ada operation kedua dan selanjutnya menggunakan nama yang sama, maka hasilnya akan ditumpang.*

> Condition 

Jika kita ingin menambahkan kondisi, misal memilih data berdasarkan kriteria tertentu. Kita bisa menggunakan `condition`. Dan perlu diperhatikan semua tambahan itu berada dibawah `instruction`, karena dilevel teratas object `operation` hanya ada `operation` dan `instruction`. Cara kita mendefinisikan kondisi itu mengikuti sebagian dokumentasi MongoDB, [Query Comparison](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/) dan [Query Logical](https://www.mongodb.com/docs/manual/reference/operator/query-logical/).

Misalnya,

```js
{
	operation: 'Select',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
		condition: {
			$and: [
				{ quarter: 'Q4' },
				{ isActive: true },
			],
		},
	},
}
```

Untuk kasus dimana kita perlu memberikan kondisi pada kolom relasi `one`, itu harus seperti ini,
```js
$and: [
	{
		faktur_return_product: {
			id: {
				$eq: 1
			}
		}
	},
	{
		items: {
			id: {
				$eq: 1
			}
		}
	},
]
```

Sedangkan untuk relasi `many` dari `one-to-many`, arahkan ke table yang `one` saja. Namun untuk `many-to-many`, lebih baik menggunakan `rawquery`.

> Populate

Kita gunakan ini untuk mengganti, misal `objective_id_items` adalah kolom relasi, awalnya bertipe `integer` dan isinya `id`, diganti dengan `object` dari row table sesuai relasi. Juga bisa kita gunakan untuk relasi `many`, muncul nanti sebagai `array of object`.

```js
{
	operation: 'Select',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
		populate: ['objective_id_items'],
	},
}
```
*Jika populate relation-many, maka pastikan tambahkan _items dibelakang nama kolomnya.*

> Chain.

Tidak terkhususkan pada `Select`, kita bisa juga melakukan pada operation lain. Digunakan untuk menggunakan hasil dari operation sebelumnya untuk operation selanjutnya. Cara mengakses hasil dari operation adalah dengan menambahkan *double curly bracket* lalu nama dari operation-nya, seperti dibawah,


```js
[{
	operation: 'Select',
	instruction: {
		table: 'TABLE_NAME',
		name: `ACTION_1`,
	},
},{
	operation: 'Select',
	instruction: {
		table: 'DIFF_TABLE_NAME',
		name: `ACTION_2`,
		condition: {
			id: `{{ACTION_1[0].id}}` // <--
		},
	},
}]
```

# Insert


```js
{
	operation: 'Insert',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
		data: {
			payload_0: 1,
		},
	},
}
```

# Delete


```js
{
	operation: 'Delete',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
		condition: {
			id: 1,
		},
	},
}
```
*Careful, if value under `condition` is null then it mean that condition never exist, thus could potentially delete whole table.*

# Update

```js
{
	operation: 'Update',
	instruction: {
		table: 'TABLE_NAME',
		name: `PER_ACTION_NAME`,
		condition: {
			id: 1,
		},
		set: {
			name: 'NEW_NAME',
		},
	},
}
```
*Careful, if value under `condition` is null then it mean that condition never exist, thus could potentially update whole table.*


