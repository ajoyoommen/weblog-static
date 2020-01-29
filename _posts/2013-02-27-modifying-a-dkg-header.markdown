---
layout: post
title: Modifying a DKG header
date: 2013-02-27 18:36:26 +0530
category: Asynchronous Key Generation for IBE
tags:
    - C++
    - DKG
author: Ajoy Oommen
published: true
---
The changes and the modifications that have been made to the networkmessage.h file :

I might change this later on, probably.

    typedef enum {
      NET_MSG_NONE, NET_MSG_PING, NET_MSG_PONG,
      VSS_SEND, VSS_ECHO, VSS_READY, VSS_SHARED, VSS_HELP,
      DKG_SEND, DKG_ECHO, DKG_READY, DKG_HELP, LEADER_CHANGE,
      IBC_REQUEST, IBC_REPLY,
      RECONSTRUCT_SHARE, PUBLIC_KEY_EXCHANGE, BLS_SIGNATURE_REQUEST,
      BLS_SIGNATURE_RESPONSE, WRONG_BLS_SIGNATURES, VERIFIED_BLS_SIGNATURES
        } NetworkMessageType;

    class IBCRequestMessage : public NetworkMessage
    {
    public:
      IBCRequestMessage(NodeID node, string &amp;ID);
      IBCRequestMessage(const Buddy *buddy, const string &amp;str, int g_recv_ID);
      NodeID node;
      string ID;
      FILE *file;
    };

    class IBCReplyMessage : public NetworkMessage
    {
    public:
      IBCReplyMessage(Zr Hidshare, NodeID selfID, NodeID recpID);
      IBCReplyMessage(const Buddy *buddy, const string &amp;str, int g_recv_ID);
      NodeID selfID;
      NodeID recpID;
      Zr share;
    };