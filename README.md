# fs-Whitelist-db-samp
//fix delay bot by faruq-
-------------------------
//VARIABLE
new DCC_Channel:AccUcp;

//PASANG DI ONGAMEMODEINIT
AccUcp = DCC_FindChannelById("id channel anda"); //id channel for whitelist

//PASANG DI ONPLAYERCONNECT
mysql_format(mMysql, format_string, 144, "SELECT * FROM `whitelist_player` WHERE `Name` = '%s'", Name(playerid));
	new Cache: result = mysql_query(mMysql, format_string);

	new rows = cache_num_rows();

	if(!rows)
		KickNoWL(playerid);

//PASANG DI BAWAH STOCK CREATEPLAYERSATIETY
stock KickNoWL(playerid)
{
	SCM(playerid, COLOR_YELLOW, "{deb545}SERVER: {ffffff}Akun Anda Belom Terdaftar Di Whitelist Atau Anda Di Blacklist!");
	format(format_string, 133, "{FFA352}Lexx's Bot: {FFFF00}%s {FF541F}Has Been Kicked From Server {FFFF00}(No Whitelist / Blacklist)", Name(playerid));
    SendClientMessageToAll(playerid, format_string);
    format(String, 766, "{FFFFFF}Akun Anda: {C82EFF}%s\n{FFFFFF}Belom Terdaftar Di Whitelist Server Ini Atau Akun Anda Di Blacklist\nAnda Dapat Menghubungi Administrator Server Ini Di Discord Kami:\n{7FD400}'Comingsoon'", Name(playerid));
    SPD(playerid, 0000, DIALOG_STYLE_MSGBOX, "Whitelist System", String, "Tutup", "");
    return KickD(playerid, "");
}

//CMDNYA PASANG DI BAGIAN CMD
DCMD:addw(user, channel, params[]) // untuk mencopot blacklist atau menambahkan whitelist warga
{
	new InfoUcp[128];
    if(channel != AccUcp) return 1;
    if(sscanf(params, "s[128]",params[0])) return DCC_SendChannelMessage(AccUcp, "!addw [nama]");

	format(format_string, 144, "INSERT INTO `whitelist_player`(`Name`) VALUES ('%s')", params[0]);
	mysql_tquery(mMysql, format_string);

	format(InfoUcp, sizeof(InfoUcp), ":pencil:**%s** ```Has Added To Whitelist```", params[0]);
	DCC_SendChannelMessage(AccUcp, InfoUcp);
	return 1;
}
DCMD:blw(user, channel, params[]) // untuk mengblacklist warga
{
	new InfoUcp[128];
    if(channel != AccUcp) return 1;
    if(sscanf(params, "s[128]",params[0])) return DCC_SendChannelMessage(AccUcp, "!addw [nama]");

	format(format_string, 144, "DELETE FROM `whitelist_player` WHERE `Name` = '%s'", params[0]);
	mysql_tquery(mMysql, format_string);

	format(InfoUcp, sizeof(InfoUcp), ":lock:**%s** ```Has Been Blacklist From Server```", params[0]);
	DCC_SendChannelMessage(AccUcp, InfoUcp);
	return 1;
}

//DATABASE PHP MYSQLNYA
CREATE TABLE `whitelist_player` (
  `Name` varchar(32) CHARACTER SET utf8 NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
