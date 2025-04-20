@startuml
skinparam dpi 400
!pragma teoz true
skinparam defaultTextAlignment center
skinparam participantFontColor white
skinparam noteFontColor white
skinparam participantBorderColor None
skinparam ArrowThickness 2
participant "中央控制系統" #dodgerblue
participant AGV #dodgerblue
participant "地圖伺服器" #dodgerblue
hide footbox

group 下載地圖
"中央控制系統" -> "AGV": 觸發即時動作 "downloadMap"
activate AGV
AGV -> "地圖伺服器": 下載地圖請求
activate "地圖伺服器"
"地圖伺服器" -> AGV: 傳遞地圖檔案
deactivate "地圖伺服器"
"AGV" -> "AGV": 如果下載成功\n檢查地圖檔案
"AGV" -> "AGV": 處理地圖檔案\n使其可使用
AGV -> "中央控制系統": 回報即時動作為完成(FINISHED)
"AGV" -> "中央控制系統": 傳送更新後的狀態\n 以及在 maps 陣列中的新地圖資訊
deactivate AGV
end

group 啟用地圖
"中央控制系統" -> "AGV": 觸發即時動作 "enableMap"
activate AGV
"AGV" -> "AGV": 將指定的 mapId 和 mapVersion \n對應之地圖設定為啟用（ENABLED）\n將相同 mapId 但不同 mapVersion \n的其他地圖設為停用（DISABLED）
"AGV" -> "中央控制系統": 傳送更新後的狀態
deactivate AGV
end

group 刪除車輛上的地圖
"中央控制系統" -> "AGV": 觸發即時動作 "deleteMap"
activate AGV
"AGV" -> "AGV": 從車輛記憶體中移除\n指定的 mapId 和 mapVersion \n對應的地圖
"AGV" -> "中央控制系統": 回報即時動作為完成(FINISHED)
"AGV" -> "中央控制系統": 傳送更新後的狀態 \nmaps 陣列中已移除該地圖
deactivate AGV
end
@enduml