#!/bin/bash

function usage {
    echo -e "
    $(basename ${0}) is a tool for ...

    Usage:
        $(basename ${0}) [command]"
}

function tmux_yesno() {
    title="found session"
    msg="Would you like to connect to an existing session?"
    yesno=$(whiptail --title "$title" --yesno "$msg" 15 80 3>&1 1>&2 2>&3)
    return $?
}

function tmux_select() {
    title="select tmux session"
    msg="please select tmux session"
    select_session=$(whiptail --notags --menu "test" 20 120 12 ${s_list[@]} 3>&1 1>&2 2>&3)
    [ $? = 0 ] && tmux a -t $select_session
}

function tmux_start(){
    title="tmux session name"
    msg="Please input yout tmux session name"
    default="$(date +%Y%m%d_%H%M%S)_session"
    session_name=$(whiptail --inputbox "$msg" 15 80 "$default" --title "$title" 3>&1 1>&2 2>&3)
    [ ! $session_name = "" ] && tmux new -s $session_name $cmd
}

cmd=$@

# get tmux session list
s_strlist=$(tmux ls -F "#{session_id} #{session_name}_#{?session_attached,(attached),}")
tmux_flg=$?
OLD_IFS=$IFS
IFS=$'\n' s_list=($s_strlist)
IFS=$OLD_IFS

echo $tmux_flg
if [ $tmux_flg = 1 ];then
    tmux_start
else 
    tmux_yesno
    flg=$?
    if [ $flg = 0 ];then
        tmux_select
    else
        tmux_start
    fi
fi
