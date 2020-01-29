---
layout: post
title: Analyzing messages in DKG
date: 2013-02-23 18:31:32 +0530
category: Asynchronous Key Generation for IBE
tags:
    - C++
    - DKG
author: Ajoy Oommen
published: true
---
In networkmessage.h, these are the message declarations for writing a message and reading the message.

PingNetworkMessage :

    class PingNetworkMessage : public NetworkMessage
    {
    public:
      PingNetworkMessage(const BuddySet &amp;buddyset, unsigned int t);
      PingNetworkMessage(const Buddy *buddy, const string &amp;str, int g_recv_ID);
      string toString() const;
      unsigned int t;
      string DSA;
      string strMsg;
      bool msgValid;
    };

In networkmessage.cc, these are the declarations.

    PingNetworkMessage::PingNetworkMessage(const BuddySet &amp;buddyset, unsigned int t): t(t) {
        string msgbody;
        //size_t signstart = msgbody.size();
        write_ui(msgbody, t);
        //size_t signend = msgbody.size();
        write_sig(buddyset, msgbody, toString());
        addMsgHeader(NET_MSG_PING, msgbody);
        addMsgID(msg_ID, msgbody);
        set_netMsgStr(msgbody);
    }

    PingNetworkMessage::PingNetworkMessage(const Buddy *buddy, const string &amp;str, int g_recv_ID):
            NetworkMessage(str) {
        const unsigned char *bodyptr = (const unsigned char *)str.data() + headerLength;
        size_t bodylen = str.size() - headerLength;
        //const unsigned char *signstart = bodyptr;
        read_ui(bodyptr, bodylen, t);
        //const unsigned char *signend = bodyptr;
        strMsg = toString();
        msgValid = read_sig(buddy, bodyptr, bodylen, (const unsigned char *)strMsg.data(),
                                    (const unsigned char *)strMsg.data()+ strMsg.length());
        DSA = str.substr(str.size()-bodylen- buddy-&gt;sig_size(), buddy-&gt;sig_size());
        msg_ID = g_recv_ID;
    }

    string PingNetworkMessage::toString() const{
        string msgStr;
        write_ui(msgStr, t);
        addMsgHeader(NET_MSG_PING, msgStr);
        return msgStr;
    }