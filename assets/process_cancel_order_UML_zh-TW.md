@startuml
skinparam dpi 400
skinparam defaultTextAlignment center
start
:AGV 有任務;
#lightgreen:AGV 接收到即時動作\n <B>cancelOrder</B>.\n 該動作被加入<B>actionStates</B>.\n 等待中的動作狀態設為 <B>FAILED</B>;
if (當前運行中的動作\n 是否可以被中斷?) then (是)
    #lightgreen: 中斷動作,\n 並將其狀態設為<B>FAILED</B>;
else (否)
    #lightgreen: 完成動作並回報其狀態\n(<B>RUNNING</B>,\n <B>FAILED</B>, <B>FINISHED</B>);
endif

if (AGV 是否能在\n 節點之間停止?) then (否)
    #lightgreen:  AGV 繼續駛向下一個節點\n 在此節點能接收新任務 \n <B>cancelOrder</B> 動作狀態設為 <B>RUNNING</B> \n 保留 AGV 駛向的節點的 nodeState \n刪除所有其他 nodeStates 和 edgeStates;
    #lightgreen: AGV 抵達停止的節點\n lastNodeId 和 lastNodeSequenceId\n 必須相應更新;
else (是)
    #lightgreen: AGV 停止;
endif
#lightgreen: 刪除所有 nodeStates 和 edgeStates \n 但保留 actionStates。 \n <B>cancelOrder</B> 動作狀態設為 <B>FINISHED</B>.;
stop
@enduml