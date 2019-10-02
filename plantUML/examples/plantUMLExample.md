@startuml
title Untag Login Sesssion
skinparam padding 10
skinparam SequenceMessageAlign left

actor VenueStaff

box "UC"
entity SessionDataTagging
endbox

box "FOBT"
entity UntagLoginSessionCommand
database Derby
end box

box "DC"
entity IdentityServer
end box

VenueStaff -> SessionDataTagging : Untag

SessionDataTagging -> UntagLoginSessionCommand : Send UntagLoginSessionCommand
activate UntagLoginSessionCommand
UntagLoginSessionCommand -> Derby : Update local dc
note left
	userId to -1
	userDb to ""
end note
UntagLoginSessionCommand -> IdentityServer : Update remote dc
note left
	Unlinks anonymous profile
	to M&R profile
end note
IdentityServer --> UntagLoginSessionCommand : Update response
UntagLoginSessionCommand --> SessionDataTagging : Command response
deactivate UntagLoginSessionCommand
@enduml