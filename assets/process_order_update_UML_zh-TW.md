```
@startuml
skinparam dpi 250
start
#lightgreen:任務透過 MQTT 送達;
#lightgreen:驗證 JSON 格式;
if ((1) 接收到的任務是否有效?) then (否)
    #orange:拒絕任務\n拋出錯誤;
    stop
endif
->是;
    if ((2) 接收到的任務是新的嗎?) then (是)
        if ((3) 車輛是否仍在執行任務\n或當前任務正在等待更新?) then (是)
        #orange:拒絕任務\n拋出錯誤;
         stop
       endif
       ->否;
            if ((4) 新任務的起點是否與\n當前位置足夠接近?) then (否)
                #orange:拒絕任務\n拋出錯誤;
                stop
            endif
            ->是;
                #lightgreen:刪除先前任務的狀態;
                #lightgreen:- 接受任務\n- 設定 orderId 和 orderUpdateId\n- 填入新任務的狀態 (9);
          #lightgreen: 執行任務;
     else (否 - 接收到的任務是當前任務的更新)
        if ((5) 收到的更新任務是否已棄用?) then (是)
            #orange:拒絕任務\n拋出錯誤;
            stop
        endif
        ->否;
            if ((6) 收到的更新任務是否與車輛目前的任務相同？) then (是 - 車輛已經接收到更新)
                #lightgreen: 丟棄此訊息;
                stop
            endif
            ->否;
                if ((3) 車輛是否仍在執行任務\n或當前任務正在等待更新?) then (是)
                    if ((7) 收到的更新任務\n是否能作為\n當前運行中任務的有效延續？) then (否)
                      #orange:拒絕任務\n拋出錯誤;
                      stop
                    endif
                    ->是;
                        #lightgreen:清除預視區段（如果車輛有的話）;
                        #lightgreen:- 接受任務更新\n- 設定 orderUpdateId\n- 將狀態附加到當前運行/計劃的任務 (9);
                else (否)
                    if ((8) 收到的更新任務\n是否能作為\n已完成任務的有效延續？) then (否)
                        #orange:拒絕任務\n拋出錯誤;
                        stop;
                    endif
                    ->是;
                    #lightgreen:- 接受任務\n- 設定 orderId 和 orderUpdateId\n- 填入新任務的狀態 (9);
                endif
                #lightgreen:執行任務;
    endif
stop
@enduml
```
