May 11, 2011
JOIN in SQL UPDATE statement, differences between MS SQL and MySQL
MySQL:
view plaincopy to clipboardprint?


    UPDATE contact SET contact.company_fk = team.company_fk FROM contact
    INNER JOIN team ON team.id = contact.team_fk

MS SQL:
view plaincopy to clipboardprint?


    UPDATE contact INNER JOIN team ON team.id = contact.team_fk SET
    contact.company_fk = team.company_fk

 December 2, 2010
I'm a workaholic. I'm working on it.

December 2, 2010
bash script to display svn and/or git status

view plaincopy to clipboardprint?

    #!/usr/bin/env bash
    if [ -e ".svn" ]
    then
        svn status
    fi
    if [ -e ".git" ]
    then
        git status
        git rev-parse HEAD
    fi

November 28, 2010
Send mail from JBoss5 AS

1) edit mail-service.xml in server/default/deploy

view plaincopy to clipboardprint?

    <?xml version="1.0" encoding="UTF-8"?>
    <server>
      <mbean code="org.jboss.mail.MailService"
             name="jboss:service=Mail">
        <attribute name="JNDIName">java:/Mail</attribute>
        <attribute name="User">$$$$$$$$</attribute>
        <attribute name="Password">$$$$$$$$</attribute>
        <attribute name="Configuration">
          <configuration>
            <property name="mail.store.protocol" value="pop3"/>
            <property name="mail.transport.protocol" value="smtp"/>
            <property name="mail.user" value="nobody"/>
            <property name="mail.pop3.host" value="pop3.nosuchhost.nosuchdomain.com"/>
            <property name="mail.smtp.host" value="smtp.$$$$$$$$.de"/>
            <property name="mail.smtp.user" value="$$$$$$$$" />
            <property name="mail.smtp.password" value="$$$$$$$$" />
            <property name="mail.smtp.auth" value="true" />
            <property name="mail.smtp.port" value="587"/>
            <property name="mail.from" value="$$$$$$$$"/>
            <!-- Enable debugging output from the javamail classes -->
            <property name="mail.debug" value="true"/>
          </configuration>
        </attribute>
        <depends>jboss:service=Naming</depends>
      </mbean>
    </server>
2) call it from your java code:

view plaincopy to clipboardprint?

    private void sendMail(String fromAdr, String toAdr, String subject,
                            String msg) {
                    try {
                            Context initCtx = new InitialContext();
                            Session session = (Session) initCtx.lookup("java:/Mail");

                            Message message = new MimeMessage(session);
                            message.setFrom(new InternetAddress(fromAdr));
                            InternetAddress to[] = new InternetAddress[1];
                            to[0] = new InternetAddress(toAdr);
                            message.setRecipients(Message.RecipientType.TO, to);
                            message.setSubject(subject);
                            message.setContent(msg, "text/plain");
                            Transport.send(message);
                    } catch (Exception e) {
                            e.fillInStackTrace();
                    }
            }

Sources:- http://community.jboss.org/thread/121612- http://mrmcgeek.blogspot.com/2010/09/confguring-java-mail-with-jboss-as-5.html- http://achorniy.wordpress.com/2009/12/15/jboss-mail-service-with-ssl-smtp/

