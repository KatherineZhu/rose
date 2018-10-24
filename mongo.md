# Mongo

## mongodump and mongorestore

db.chatMessages.createIndex({app:1,"content.extra.accountId":1,"content.extra.action":1,createdAt:-1},{background:true})
db.chatMessages.find({app:ObjectId("54d04f4605b874fd7942fc06"),"content.extra.accountId":"5628a3ffba1b822c578b45e5","content.extra.action":"chat",createdAt:{$gt:ISODate("2018-09-06T16:00:00.000Z"),$lt:ISODate("2018-09-07T16:00:00.000Z")}}).explain("executionStats").executionStats

mkdir ~/katherine.zhu/danone-201809131125/

mongodump -u tuisongbao -p tsbawesome! -h dds-bp1dc38be32c80a41.mongodb.rds.aliyuncs.com:3717 -d tuisongbao_www -c chatMessages -o ~/katherine.zhu/danone-201809131125/ --query '{app:ObjectId("54d04f4605b874fd7942fc06"),"content.extra.accountId":"5628a3ffba1b822c578b45e5","content.extra.action":"chat",createdAt:{$gt:ISODate("2018-09-06T16:00:00.000Z"),$lt:ISODate("2018-09-07T16:00:00.000Z")}}'

scp root@tsb-app-3:~/katherine.zhu/danone-201809131125 ~/workspace/tuisongbao/dump

mongorestore -u root -p root -h 192.168.222.22 --port 26017 -d portal-tenants-shared -c chatMessages ~/workspace/tuisongbao/dump/danone-201809131125/tuisongbao_www/chatMessages.bson

## Index

We have {a:1,b:1} and {a:1,c:1,b:1}, if we `find(a).sort(b)`, they have different stage:

- {a:1,b:1} only FETCH

```
scrm-staging:PRIMARY> db.chatSession.find({accountId:ObjectId("54bf3b4c13747372268b4567")}).sort({createdAt:1}).hint("accountId_1_createdAt_-1").explain("executionStats")
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "portal-tenants-qa.chatSession",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"accountId" : {
				"$eq" : ObjectId("54bf3b4c13747372268b4567")
			}
		},
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"accountId" : 1,
					"createdAt" : -1
				},
				"indexName" : "accountId_1_createdAt_-1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "backward",
				"indexBounds" : {
					"accountId" : [
						"[ObjectId('54bf3b4c13747372268b4567'), ObjectId('54bf3b4c13747372268b4567')]"
					],
					"createdAt" : [
						"[MinKey, MaxKey]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 16305,
		"executionTimeMillis" : 23,
		"totalKeysExamined" : 16305,
		"totalDocsExamined" : 16305,
		"executionStages" : {
			"stage" : "FETCH",
			"nReturned" : 16305,
			"executionTimeMillisEstimate" : 30,
			"works" : 16306,
			"advanced" : 16305,
			"needTime" : 0,
			"needYield" : 0,
			"saveState" : 127,
			"restoreState" : 127,
			"isEOF" : 1,
			"invalidates" : 0,
			"docsExamined" : 16305,
			"alreadyHasObj" : 0,
			"inputStage" : {
				"stage" : "IXSCAN",
				"nReturned" : 16305,
				"executionTimeMillisEstimate" : 30,
				"works" : 16306,
				"advanced" : 16305,
				"needTime" : 0,
				"needYield" : 0,
				"saveState" : 127,
				"restoreState" : 127,
				"isEOF" : 1,
				"invalidates" : 0,
				"keyPattern" : {
					"accountId" : 1,
					"createdAt" : -1
				},
				"indexName" : "accountId_1_createdAt_-1",
				"isMultiKey" : false,
				"isUnique" : false,
				"isSparse" : false,
				"isPartial" : false,
				"indexVersion" : 1,
				"direction" : "backward",
				"indexBounds" : {
					"accountId" : [
						"[ObjectId('54bf3b4c13747372268b4567'), ObjectId('54bf3b4c13747372268b4567')]"
					],
					"createdAt" : [
						"[MinKey, MaxKey]"
					]
				},
				"keysExamined" : 16305,
				"dupsTested" : 0,
				"dupsDropped" : 0,
				"seenInvalidated" : 0
			}
		}
	},
	"serverInfo" : {
		"host" : "192-168-2-140",
		"port" : 27017,
		"version" : "3.2.19",
		"gitVersion" : "a9f574de6a566a58b24d126b44a56718d181e989"
	},
	"ok" : 1
}
```

- {a:1,c:1,b:1} SORT and FETCH

```
scrm-staging:PRIMARY> db.chatSession.find({accountId:ObjectId("54bf3b4c13747372268b4567")}).sort({createdAt:1}).hint("accountId_1_client.nick_1_createdAt_-1").explain("executionStats")
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "portal-tenants-qa.chatSession",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"accountId" : {
				"$eq" : ObjectId("54bf3b4c13747372268b4567")
			}
		},
		"winningPlan" : {
			"stage" : "SORT",
			"sortPattern" : {
				"createdAt" : 1
			},
			"inputStage" : {
				"stage" : "SORT_KEY_GENERATOR",
				"inputStage" : {
					"stage" : "FETCH",
					"inputStage" : {
						"stage" : "IXSCAN",
						"keyPattern" : {
							"accountId" : 1,
							"client.nick" : 1,
							"createdAt" : -1
						},
						"indexName" : "accountId_1_client.nick_1_createdAt_-1",
						"isMultiKey" : false,
						"isUnique" : false,
						"isSparse" : false,
						"isPartial" : false,
						"indexVersion" : 1,
						"direction" : "forward",
						"indexBounds" : {
							"accountId" : [
								"[ObjectId('54bf3b4c13747372268b4567'), ObjectId('54bf3b4c13747372268b4567')]"
							],
							"client.nick" : [
								"[MinKey, MaxKey]"
							],
							"createdAt" : [
								"[MaxKey, MinKey]"
							]
						}
					}
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 16304,
		"executionTimeMillis" : 94,
		"totalKeysExamined" : 16304,
		"totalDocsExamined" : 16304,
		"executionStages" : {
			"stage" : "SORT",
			"nReturned" : 16304,
			"executionTimeMillisEstimate" : 86,
			"works" : 32611,
			"advanced" : 16304,
			"needTime" : 16306,
			"needYield" : 0,
			"saveState" : 255,
			"restoreState" : 255,
			"isEOF" : 1,
			"invalidates" : 0,
			"sortPattern" : {
				"createdAt" : 1
			},
			"memUsage" : 17673660,
			"memLimit" : 33554432,
			"inputStage" : {
				"stage" : "SORT_KEY_GENERATOR",
				"nReturned" : 0,
				"executionTimeMillisEstimate" : 40,
				"works" : 16306,
				"advanced" : 0,
				"needTime" : 1,
				"needYield" : 0,
				"saveState" : 255,
				"restoreState" : 255,
				"isEOF" : 1,
				"invalidates" : 0,
				"inputStage" : {
					"stage" : "FETCH",
					"nReturned" : 16304,
					"executionTimeMillisEstimate" : 20,
					"works" : 16305,
					"advanced" : 16304,
					"needTime" : 0,
					"needYield" : 0,
					"saveState" : 255,
					"restoreState" : 255,
					"isEOF" : 1,
					"invalidates" : 0,
					"docsExamined" : 16304,
					"alreadyHasObj" : 0,
					"inputStage" : {
						"stage" : "IXSCAN",
						"nReturned" : 16304,
						"executionTimeMillisEstimate" : 0,
						"works" : 16305,
						"advanced" : 16304,
						"needTime" : 0,
						"needYield" : 0,
						"saveState" : 255,
						"restoreState" : 255,
						"isEOF" : 1,
						"invalidates" : 0,
						"keyPattern" : {
							"accountId" : 1,
							"client.nick" : 1,
							"createdAt" : -1
						},
						"indexName" : "accountId_1_client.nick_1_createdAt_-1",
						"isMultiKey" : false,
						"isUnique" : false,
						"isSparse" : false,
						"isPartial" : false,
						"indexVersion" : 1,
						"direction" : "forward",
						"indexBounds" : {
							"accountId" : [
								"[ObjectId('54bf3b4c13747372268b4567'), ObjectId('54bf3b4c13747372268b4567')]"
							],
							"client.nick" : [
								"[MinKey, MaxKey]"
							],
							"createdAt" : [
								"[MaxKey, MinKey]"
							]
						},
						"keysExamined" : 16304,
						"dupsTested" : 0,
						"dupsDropped" : 0,
						"seenInvalidated" : 0
					}
				}
			}
		}
	},
	"serverInfo" : {
		"host" : "192-168-2-140",
		"port" : 27017,
		"version" : "3.2.19",
		"gitVersion" : "a9f574de6a566a58b24d126b44a56718d181e989"
	},
	"ok" : 1
}
```


