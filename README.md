# just-another
testing

set folder_to_delete to ((path to library folder from user domain as text) & "Application Support:WhiteSmoke")
set pref_file to ((path to "pref" as text) & "com.WhiteSmoke.WhiteSmokeApp.plist")
set application_folder_to_delete to ((path to applications folder as text) & "WhiteSmoke")
set receipts_folder_to_delete to ((path to library folder from user domain as text) & "Receipts")

set removeString to "The application was uninstalled successfully. "


tell application "System Events"
	set myList to (name of every process)
end tell

if (myList contains "WhiteSmokeMac") is true then
	tell application "Finder" to set appPath to name of application file id "com.WhiteSmoke.WhiteSmokeApp" as string
	tell application appPath to quit
end if

delay 1 -- adjust if necessary

tell application "System Events"
	set myList to (name of every UI element of list 1 of process "Dock")
end tell

if (myList contains "WhiteSmokeMac") is true then
	try
		tell application "System Events" to tell UI element "WhiteSmokeMac" of list 1 of process "Dock"
			try
				if (exists) then
					perform action "AXShowMenu"
					click menu item "Options" of menu 1
					click menu item "Remove from Dock" of menu 1 of menu item "Options" of menu 1
				end if
			end try
		end tell
	end try
end if

tell application "Finder"
	
	if exists folder application_folder_to_delete then
		move application_folder_to_delete to trash
	end if
	if exists folder folder_to_delete then
		try
			move folder_to_delete to trash
		end try
	end if
	if exists file pref_file then
		try
			delete file pref_file -- moves it to the trash
		end try
	end if
	
	if exists folder receipts_folder_to_delete then
		repeat with myFile in folder ((path to library folder from user domain as text) & "Receipts")
			if name of myFile contains "WhiteSmoke" then
				delete myFile
			end if
		end repeat
	end if
	
	--get the name of every file in folder ((path to library folder from user domain as text) & "Receipts")
	--if file name contains string "WhiteSmoke" then
	--	move file to trash
	--end if
	
	tell application "System Events"
		--Find out what login items we have
		try
			get the name of every login item
			--see if the item we want exists.  If so then delete it
			if login item "WhiteSmokeMac" exists then
				delete login item "WhiteSmokeMac"
			end if
		end try
	end tell
	set mystring to removeString
	display dialog mystring buttons {"OK"} giving up after 10
	
	
end tell




