   # OpenId Connect Token forwarding to embedded iFrame
     
   * Status: open
   * Deciders: Christian Schulz, Ömer Yildiz, Marius Schulze
   * Date: 2020-09-23
     
   Technical Story: https://tfs.pmcs-helpline.com/tfs/Helpline/HelpLineScrum/_workitems/edit/209123
     
   ## Context and Problem Statement  
     
   When embedding the messaging UI via an iFrame, how can we pass the OpenId connect token to the messaging component? The user should not need to login again. 
     
   ## Decision Drivers 
     
   * Common POWA infrastructure
   * Security concerns on how to pass the oic token in a secure manner
     
   ## Considered Options  
     
   #### Option 1 
   Event-based communication with Window.postMessage()
     
   #### Option 2   
   Passing the token via URL        

   ## Decision Outcome  

   The [Option 1](#option-1) is the recommended way to communicate securely between a page and an embedded iFrame. We will stick to this approach as it is already used in production by Messaging.
     
   ## Pros and Cons of the Options  
     
   ### [Option 1](#option-1)  
     
   The token gets passed to the embedded module via [Window.postMessage()](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). Inside of the iFrame the Messaging module listens to the message bus and verifies if the event origin is trusted to validate the sender's identity.
     
   + Good, because it is the recommended way to communicate between a page and an iFrame.
   + Good, because it is already used by Messaging in production.

   The data structure for the serialized message will be:
    
    interface SerializedMessage {
        type: string;
        data: Record<string, any>;
    }

   The type that was agreed on with the Messaging team is:

    TOKEN_CHANGE_TYPE = 'TokenChange';

   ### [Option 2](#option-2)  
     
   The token gets passed as a query parameter of the url to the iFrame.
     
   + Good, because the implementation is easy and the current prototype already contains this functionality.
   - Bad, because the token is then easily interceptable.   