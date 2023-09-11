```sql
--搜尋 案件名稱
--SELECT id FROM [ctbc].[dbo].[ticket] where name = 'asdad'

--搜尋 提案名稱
SELECT * FROM ticket where id IN (SELECT ticket_has_ticket_type_tag.ticket_id AS id FROM ticket_type_tag  
LEFT JOIN ticket_has_ticket_type_tag ON ticket_has_ticket_type_tag.ticket_type_tag_id = ticket_type_tag.id 
WHERE ticket_type_tag.name IN ('新系統'))
SELECT id FROM ticket where name = '測試次標題'
--搜尋 提案類型
SELECT ticket_has_tag.ticket_id FROM flow_tag 
LEFT JOIN ticket_has_tag ON ticket_has_tag.flow_tag_id = flow_tag.id 
WHERE flow_tag.tag_name = 'Test77'

--搜尋 提案內容
SELECT ticket_has_ticket_type_tag.ticket_id AS id FROM ticket_type_tag  
LEFT JOIN ticket_has_ticket_type_tag ON ticket_has_ticket_type_tag.ticket_type_tag_id = ticket_type_tag.id 
WHERE ticket_type_tag.name IN ('新系統');

SELECT
  JSON_VALUE(, '$.property_name') AS extracted_property
FROM
  your_table
WHERE
  JSON_VALUE(json_column, '$.property_name') = 'some_value';
--依照ticket id 刪除 /import-tickets
--DELETE FROM time_limit_issue WHERE ticket_id IN (SELECT id FROM [ctbc].[dbo].[ticket] where name = 'asdad');
--DELETE FROM ticket WHERE id IN (SELECT id FROM [ctbc].[dbo].[ticket] where name = 'asdad');
--DELETE FROM ticket_has_tag WHERE ticket_id IN (123);
--DELETE FROM ticket_has_ticket_type_tag WHERE ticket_id IN (123);
```

```bash
curl -X 'PUT' \
  'http://150.230.202.166:10001/v1/updateMeetingFileToMeetingFilesForTicket' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJwZXJtaXNzaW9ucyI6WyJCVUlMRElOR19CTE9DS19FRElUIiwiVFFfVklFVyIsIlVTRVJfTUFOQUdFIiwiVElDS0VUX1VQTE9BRF9GSUxFIiwiQ1BfQVJUSUNMRV9NQU5BR0UiLCJURUNIX1NUQVRVU19FRElUIiwiVElDS0VUX0FERF9QUk9DRVNTT1IiLCJNQUlOX01FTlVfTUFOQUdFIiwiSU5UUk9EVUNUSU9OX0VESVQiLCJUUV9FRElUIiwiVElDS0VUX0NPUFkiLCJWSVNJT05fTUFOQUdFIiwiTkZSX0VESVQiLCJIT01FX0FSVElDTEVfTUFOQUdFIiwiQ1BfQ09NUE9ORU5UX0VESVQiLCJUSUNLRVRfQ0FOQ0VMIiwiUk9MRV9DVVNUT00iLCJUSUNLRVRfUkVTQU5EIiwiV0FGX0VESVQiLCJBRFJfRURJVCIsIlRJQ0tFVF9SRVRVUk4iLCJUSUNLRVRfU0VDUkVUQVJZIiwiTUlEX0lORk9fVklFVyIsIlRJTUVfTElNSVRfRURJVCIsIlNZU1RFTV9JTkZPX1ZJRVciLCJCVUlMRElOR19CTE9DS19WSUVXIiwiVElDS0VUX0RFTEVURV9GSUxFIiwiVElDS0VUX01PRElGWV9ERUFETElORSIsIlRFQ0hfSU5GT19FRElUIiwiUk9MRV9BRE1JTiIsIkFSQl9FRElUIl0sImVtcGxveWVlX2lkIjoiYWRtaW4iLCJpc3MiOiJjdGJjIiwiZXhwIjoxNjkzOTc4NDg2LCJpYXQiOjE2OTMzNzM2ODYsInVzZXJuYW1lIjoiYWRtaW4ifQ.7iG1aC1plyHFEY8w-cxlkCKJGWTr0GUzcEehMNIyAvM'
```

``` sh
curl -X "PUT" "http://{IP}:10001/v1/updateMeetingFileToMeetingFilesForTicket" -H "accept: */*" -H "Authorization: Bearer "
```
@All 明天上版流程

```sh
git add .
git commit -m '{當天日期} v.{版本號} update'
git push origin test
```

測試機 code 
```sh
d/eamp source
```

信箱
```
版號跟 github 版本號比對正確

```
![[assets/Pasted image 20230901105507.png]]

下載信箱code ，覆蓋到
```sh
d/eamp source
裡面
examp-agile 前端
examp-main 後端
(不要覆蓋到.git)

分支 test
git add .
git commit -m '{當天日期} v.{版本號} update'
git push origin test
```
1. 使用 Azure DevOps 上版測試機
	點進  pipline eamp_build_frontendci, eamp_build_backendci，跑 ci
	跑完兩個ci pipline，點 EAMP_PreCD->EAMP_DEPLOY 的 Parameter 輸入PreCD的日期編號

3. 在 Azure DevOps 開 PR 進 Master

4. 將 Azure DevOps Master branch 的版本 Pull 下來，Commit 到 BitBucket

5. 到 Azure DevOps 的 Build Machine 內的前 / 後端 third-party package 
	Azure DevOps pipline 寫在 Repos 裡的 file 分別是 EAMP_BUILD_Frontend，Backend
	third-part package
	/var/lib/node_modules.zip
	後端 .m2 (如果有新套件)
	/root/.m2 /
	切換 admin,密碼 拆信封
	`243` Azure Devops build版機
	資料先放在 181/owner 公號
	`241` jenkins build 版機
搬到 Jankins Build Slave 上對應的位置，換最高權限登入 jenkins build才能傳輸
jenkins 的 pipline DVPSCD_SCRIPT EAMP-main 看到 `third-part` 的部屬目錄
chown apuser:apuser ./{.zip}
8. 用 Jenkins CICD 上版正式機
	EAMP_BUILD
	前端後端 都要使用 Bitbucket 的 commit key 寫在 build with parameter
所以結論會是明天 Azure DevOps master 的版本 會跟 BitBucket 一致


```sh
curl -X 'PUT' \
  'http://localhost:10001/v1/ticket-data/department-name' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer e' \
  -H 'Content-Type: application/json' \
  -d '[
  {
    "id": 4055,
    "department_name": "string1231231231"
  }
]'



curl -X "PUT" "http://localhost:10001/v1/ticket-data/department-name" -H "accept: */*" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJwZXJtaXNzaW9ucyI6WyJUSUNLRVRfRURJVCIsIkJVSUxESU5HX0JMT0NLX0VESVQiLCJUUV9WSUVXIiwiVVNFUl9NQU5BR0UiLCJUSUNLRVRfVVBMT0FEX0ZJTEUiLCJDUF9BUlRJQ0xFX01BTkFHRSIsIlRFQ0hfU1RBVFVTX0VESVQiLCJUSUNLRVRfQUREX1BST0NFU1NPUiIsIk1BSU5fTUVOVV9NQU5BR0UiLCJJTlRST0RVQ1RJT05fRURJVCIsIlRRX0VESVQiLCJUSUNLRVRfQ09QWSIsIlZJU0lPTl9NQU5BR0UiLCJORlJfRURJVCIsIkhPTUVfQVJUSUNMRV9NQU5BR0UiLCJDUF9DT01QT05FTlRfRURJVCIsIlRJQ0tFVF9DQU5DRUwiLCJST0xFX0NVU1RPTSIsIlRJQ0tFVF9SRVNBTkQiLCJXQUZfRURJVCIsIkFEUl9FRElUIiwiVElDS0VUX1JFVFVSTiIsIlRJTUVfTElNSVRfRURJVCIsIkJVSUxESU5HX0JMT0NLX1ZJRVciLCJUSUNLRVRfREVMRVRFX0ZJTEUiLCJUSUNLRVRfTU9ESUZZX0RFQURMSU5FIiwiVEVDSF9JTkZPX0VESVQiLCJST0xFX0FETUlOIiwiQVJCX0VESVQiXSwiZW1wbG95ZWVfaWQiOiJhZG1pbiIsImlzcyI6ImN0YmMiLCJleHAiOjE2OTQ1MDk3MjksImlhdCI6MTY5MzkwNDkyOSwidXNlcm5hbWUiOiJhZG1pbiJ9.4yBUaa9StzheyrXNOGT4Rt2_Do-THspdnGhpj8L-12Y" -H "Content-Type: application/json" -d "[ { \"id\": 4055, \"department_name\": \"strittttt\" }]"

curl -X "PUT" "http://localhost:10001/v1/ticket-data/department-name" -H "accept: */*" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJwZXJtaXNzaW9ucyI6WyJUSUNLRVRfRURJVCIsIkJVSUxESU5HX0JMT0NLX0VESVQiLCJUUV9WSUVXIiwiVVNFUl9NQU5BR0UiLCJUSUNLRVRfVVBMT0FEX0ZJTEUiLCJDUF9BUlRJQ0xFX01BTkFHRSIsIlRFQ0hfU1RBVFVTX0VESVQiLCJUSUNLRVRfQUREX1BST0NFU1NPUiIsIk1BSU5fTUVOVV9NQU5BR0UiLCJJTlRST0RVQ1RJT05fRURJVCIsIlRRX0VESVQiLCJUSUNLRVRfQ09QWSIsIlZJU0lPTl9NQU5BR0UiLCJORlJfRURJVCIsIkhPTUVfQVJUSUNMRV9NQU5BR0UiLCJDUF9DT01QT05FTlRfRURJVCIsIlRJQ0tFVF9DQU5DRUwiLCJST0xFX0NVU1RPTSIsIlRJQ0tFVF9SRVNBTkQiLCJXQUZfRURJVCIsIkFEUl9FRElUIiwiVElDS0VUX1JFVFVSTiIsIlRJTUVfTElNSVRfRURJVCIsIkJVSUxESU5HX0JMT0NLX1ZJRVciLCJUSUNLRVRfREVMRVRFX0ZJTEUiLCJUSUNLRVRfTU9ESUZZX0RFQURMSU5FIiwiVEVDSF9JTkZPX0VESVQiLCJST0xFX0FETUlOIiwiQVJCX0VESVQiXSwiZW1wbG95ZWVfaWQiOiJhZG1pbiIsImlzcyI6ImN0YmMiLCJleHAiOjE2OTQ1MDk3MjksImlhdCI6MTY5MzkwNDkyOSwidXNlcm5hbWUiOiJhZG1pbiJ9.4yBUaa9StzheyrXNOGT4Rt2_Do-THspdnGhpj8L-12Y" -H "Content-Type: application/json" -d "[ { \"id\": 4055, \"department_name\": \"時限\" }, { \"id\": 4039, \"department_name\": \"時限\" }, { \"id\": 4066, \"department_name\": \"時限\" }]"



curl -X "PUT" "http://localhost:10001/v1/ticket-data/department-name" -H "accept: */*" -H "Authorization: Bearer eyJhbGci" -H "Content-Type: application/json" -d "[ { \"id\": 4055, \"department_name\": \"時限\" }, { \"id\": 4039, \"department_name\": \"時限\" }, { \"id\": 4066, \"department_name\": \"時限\" }]"




版塊
	領域
		業務組件
			微服務
```

[[中信上板]]