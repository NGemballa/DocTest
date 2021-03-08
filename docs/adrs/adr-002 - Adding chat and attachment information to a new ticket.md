   # Adding chat and attachment information to a new ticket
     
   * Status: accepted
   * Deciders: Christian Schulz, Ömer Yildiz, Victor Mosin, Marc Schmidt, Nima Hamidian, Philip Koep, Jens Schmitz, Marius Schulze
   * Date: 2020-10-06
     
   Technical Story: https://tfs.pmcs-helpline.com/tfs/Helpline/HelpLineScrum/_workitems/edit/202886
     
   ## Context and Problem Statement  
     
   When the agent creates a new ticket out of a chat, how is the chat added to the new ticket? Will it be visible to the agent or will it be added on the server-side?
     
   ## Decision Drivers  
     
   * The chat content and especially the attachments can have a decent storage size. When integrated on the client it will need data volume to transfer them from the messaging backend to the processes client to send it to the processes backend.
     
   ## Considered Options  
     
   #### Option 1 
   Client-side integration (Message Bus)
     
   #### Option 2   
   Client-side integration (API call)
     
   #### Option 3   
   Server-side integration     
     
   ## Decision Outcome  
     
   Chosen option: [Option 3](#option-3), because large content doesn't need to be transferred to the client but is queried via server-server communication. 
     
   ## Pros and Cons of the Options  
     
   ### [Option 1](#option-1)  
     
   The messaging component receives the chat content and attachments from the embedded iframe via event-based communication. It will display the chat inside the first multiline text field and the attachments are added to the first attachment control. 
     
   + Good, because the agent can see and modify the chat content when creating a ticket.       
   - Bad, because lots of data has to be transferred via events which might not be the intended use case.
   - Bad, because a lot of data volume would be consumed.
     
   ### [Option 2](#option-2)  
     
   The chat content and attachments are loaded via an API from the messaging backend instead of an event-based communication.
     
   + Good, because the agent can see and modify the chat content when creating a ticket.
   + Good, because an API call is better for loading data from the backend than passing it via events.   
   - Bad, because a lot of data volume would be consumed.   
     
   ### [Option 3](#option-3)  
     
   The chat content and the attachments are exchanged between the processes server and messaging server. The client only displays information that the chat and attachments will be added to the ticket automatically. The POWA client receives the current chat ID from the iframe. The processes server queries the needed content from the messaging server. The attachments could be added only via a link so there is no data duplication. This needs to be decided in another ADR. 
     
   + Good, because the client doesn't need to handle large data. 
   + Good, because server-to-server communication would be done asynchronous so the agent doesn't have to wait for the data to be loaded.
   - Bad, because the agent doesn't see the chat in the popup dialog.     
     