Download Link: https://assignmentchef.com/product/solved-cpsc351-assignment-1
<br>
This assignment has the following goals:

<ol>

 <li>To solidify your understanding of <strong>IPC principles</strong>.</li>

 <li>To develop greater appreciation for the different <strong>IPC mechanisms</strong>.</li>

 <li>To gain hands-on experience using <strong>shared memory</strong>.</li>

 <li>To gain hands-on experience using <strong>message queues</strong>.</li>

 <li>To gain hands-on experience using <strong>signals</strong>.</li>

 <li>To learn how to combine shared memory and message queues in order to implement a practical application where <strong>the sender process sends information to the receiver process</strong>.</li>

</ol>

<strong>Overview </strong>

In this assignment you will use your knowledge of shared memory and message queues in order to implement an application which synchronously transfers files between two processes.

You shall implement two related programs: a <em>sender program </em>and the <em>receiver program </em>as described below:

<ul>

 <li><strong>sender: </strong>this program shall implement the process that sends files to the receiver process. It shall perform the following sequence of steps:</li>

</ul>

<ol>

 <li>The sender shall be invoked as ./sender file.txt where sender is the name of the executable and file.txt is the name of the file to transfer.</li>

 <li>The program shall then attach to the shared memory segment, and connect to the message queue both previously set up by the receiver.</li>

 <li>Read a predefined number of bytes from the specified file, and store these bytes in the chunk of shared memory.</li>

 <li>Send a message to the receiver (using a message queue). The message shall contain a field called size indicating how many bytes were read from the file.</li>

 <li>Wait on the message queue to receive a message from the receiver confirming success- ful reception and saving of data to the file by the receiver.</li>

 <li>Go back to step 3. Repeat until the whole file has been read.</li>

 <li>When the end of the file is reached, send a message to the receiver with the size field set to 0. This will signal to the receiver that the sender will send no more.</li>

 <li>Close the file, detach shared memory, and exit.</li>

</ol>

<ul>

 <li><strong>receiver: </strong>this program shall implement  the process that  receives files from the sender process. It shall perform the following sequence of steps:</li>

</ul>

<ol>

 <li>The program shall be invoked as ./recv where recv is the name of the executable.</li>

 <li>The program shall setup a chunk of shared memory and a message queue.</li>

 <li>The program shall wait on a message queue to receive a message from the sender program. When the message is received, the message shall contain a field called size denoting the number of bytes the sender has saved in the shared memory chunk.</li>

 <li>If size is not 0, then the receiver reads size  number of bytes from shared memory, saves them to the file (always called recvfile), sends message to the sender acknowl- edging successful reception and saving of data, and finally goes back to step 3.</li>

 <li>Otherwise, if size field is 0, then the program closes the file, detaches the shared memory, deallocates shared memory and message queues, and exits.</li>

</ol>

<ul>

 <li><strong>When user presses</strong> Control-C in order to terminate the receiver, the receiver shall deallocate memory and the message queue and then exit. This can be implemented by setting up a signal handler for the SIGINT signal. Sample file illustrating how to do this have been provided (signaldemo.cpp).</li>

</ul>

<strong>Technical Details </strong>

The skeleton codes for sender and receiver can be found on Titanium.  The files are as follows:

<ul>

 <li>cpp: the skeleton code for the sender (see the TODO: comments in order to find out what to fill in)</li>

 <li>cpp: the skeleton code for the receiver (see the TODO: comments in order to find out what to fill in).</li>

 <li>h: the header file used by both the sender and the receiver</li>

</ul>

<em>  </em>It contains the struct of the message relayed through message queues).  The struct   contains two fields:

<ul>

 <li>long mtype: represents the message type.</li>

 <li>int size: the number of bytes written to the shared memory.</li>

</ul>

In addition to the structure, msg.h defines macros representing two different message         types:

<ul>

 <li>SENDER_DATA_TYPE: macro representing the message sent from sender to receiver. It’s type is 1.</li>

 <li>RECV_DONE_TYPE: macro representing the message sent from receiver to the sender acknowledging successful reception and saving of data.</li>

 <li><strong>NOTE: both message types have the same structure. The difference is how the </strong>mtype <strong>field is set. Also, the messages of type </strong>RECV_DONE_TYPE do<strong> not make use of the </strong>size</li>

 <li>Makefile: enables you to build both sender and receiver by simply typing make at the command line.</li>

 <li>cpp: a program illustrating how to install a signal handler for SIGSTP signal sent to the process when user presses Control-C.</li>

</ul>




<strong>Please note:  by default the skeleton programs will  give you errors when you run them.  This is because they are accessing unallocated, unattached regions of shared memory.   It’s your job to fill in the appropriate functionality in the skeleton, de-noted by the TODO comments, in order to make the programs work</strong>.

The following links provide additional documentation about shared memory and message queues:

<ul>

 <li>Message Queues: <a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">https://www.geeksforgeeks.org/ipc</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">–</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">using</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">–</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">message</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">–</a><a href="https://www.geeksforgeeks.org/ipc-using-message-queues/">queues/</a></li>

 <li>Shared Memory: <a href="https://www.geeksforgeeks.org/ipc-shared-memory/">https://www.geeksforgeeks.org/ipc</a><a href="https://www.geeksforgeeks.org/ipc-shared-memory/">–</a><a href="https://www.geeksforgeeks.org/ipc-shared-memory/">shared</a><a href="https://www.geeksforgeeks.org/ipc-shared-memory/">–</a><a href="https://www.geeksforgeeks.org/ipc-shared-memory/">memory/</a></li>

</ul>

<strong>Bonus  </strong>

Implement <strong><em>separate </em></strong>versions of sender and receiver which instead of relying on message queues to signal events, rely purely on signals. Hint:  use handlers for SIGUSR1 and SIGUSR2.  Hint: how can you tell the receiver the number of bytes in the shared memory?





