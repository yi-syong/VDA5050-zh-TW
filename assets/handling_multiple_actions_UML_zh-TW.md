@startuml
skinparam dpi 400
skinparam defaultTextAlignment center
start
:AGV 到達具有多個動作的節點;
#lightgreen:並行執行(動作)的清單 = [];
#lightgreen:迭代動作清單;
repeat
    if (動作:\n 阻塞類型) then (HARD)
        #lightgreen: 停止行駛;
        if (並行\n 執行清單\n 為空?) then(是)
        else(否)
        #lightgreen: 執行\n並行執行清單中\n的動作;
        endif
        #lightgreen: 執行 HARD (強阻塞)動作;
    else
        if () then(SOFT)
        #lightgreen: 停止行駛;
        else(NONE)
        endif
        #lightgreen: 將動作加入\n並行執行清單;
    endif
repeat while (節點上還有\n其他動作嗎?) is (是) not (否)
if (並行\n 執行清單\n 為空?) then(是)

else(否)
#lightgreen: 執行\n並行執行清單中\n的動作;
endif
#lightgreen: 繼續處理\n 任務;
@enduml