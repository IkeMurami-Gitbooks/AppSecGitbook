# JBoss (WildFly)

Сервер от RedHat.&#x20;

До 6 версии в JBoss EAP/AS <= `v6.Final` есть RCE: если открыты порты 4446 (JBoss Remoting Unified Invoker) или 3873 (EJB Remoting Connector) (по умолчанию открыты в старых версиях).&#x20;
