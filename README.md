# Code-though
SHARED.LUA
GM.Name		= "Ghosthunt Multiplayer" 
GM.Author	= "eburgwell" 
GM.Email	= "alderburg@gmail.com" 
DeriveGamemode( "deathmatch" )
 
team.SetUp( 1, "Guest", Color( 125, 125, 125, 255 ) ) 
team.SetUp( 2, "Admin", Color( 255, 255, 255, 255 ) ) 
team.SetUp( 3, "eburgwell", Color( 148, 0, 211, 255 ) )  
team.SetUp( 4, "Joining", Color( 0, 0 , 0, 255 ) ) 

INIT.LUA
AddCSLuaFile( "cl_init.lua" ) 
AddCSLuaFile( "shared.lua" ) 
AddCSLuaFile( "specialchars.lua" ) 
 
 
include( 'shared.lua' ) 
include( 'specialchars.lua' ) 
 
 
function GM:PlayerSpawn( ply )  
 
    self.BaseClass:PlayerSpawn( ply )  
    ply:SetGravity( 0.75 )  
    ply:SetMaxHealth( 100, true )  
 
    ply:SetWalkSpeed( 325 )  
	ply:SetRunSpeed( 325 ) 
 
end
 
function GM:PlayerInitialSpawn( ply )  
	CheckSpecialCharacters( ply ) 
	if ply:IsAdmin() then 
		sb_team2( ply ) 
	else 
	joining( ply ) 
	RunConsoleCommand( "sb_start" )	 
	end 
end 
 
 
function GM:PlayerLoadout( ply ) 
 
	if ply:Team() == 1 then 
 
		ply:Give( "weapon_flashlight" ) 
		
 
 
	elseif ply:Team() == 2 then 
 
		ply:Give( "weapon_flashlight" )
		
	
	end 
end 
 
function sb_team1( ply ) 
 
	ply:UnSpectate() 
	ply:SetTeam( 1 ) 
	ply:Spawn() 
	ply:PrintMessage( HUD_PRINTTALK, "[SimpleBuild]Welcome to the server, " .. ply:Nick() ) 
 
end 
 
 
function sb_team2( ply ) 
	
	ply:SetTeam( 2 ) 
	ply:Spawn() 
	ply:PrintMessage( HUD_PRINTTALK, "[SimpleBuild]I recognize you as an admin, " .. ply:Nick() ) 
	 
 
end 
concommand.Add( "sb_team1", sb_team1 ) 
 
function joining( ply )   
 
	ply:Spectate( 5 ) 
	ply:SetTeam( 4 ) 
 
end 

CL_INIT.LUA
include( 'shared.lua' ) 
 
function set_team() 
 
Ready = vgui.Create( "DFrame" ) 
Ready:SetPos( ScrW() / 2, ScrH() / 2 ) 
Ready:SetSize( 175, 75 ) 
Ready:SetTitle( "Are You Ready To Hunt?" ) 
Ready:SetVisible( true )  
Ready:SetDraggable( false ) 
Ready:ShowCloseButton( false ) 
Ready:MakePopup(true) 
 
ready1 = vgui.Create( "DButton", Ready ) 
ready1:SetPos( 20, 25 ) 
ready1:SetSize( 140, 40 ) 
ready1:SetText( "Hell yeah!" )  
ready1.DoClick = function() 
RunConsoleCommand( "sb_team1" ) 
 
end 
 
end 
 
concommand.Add( "sb_start", set_team ) 
