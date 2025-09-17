# syncMacApp
A Private Cross-Network File Synchronization Engine

# Introduction
synMac is a robust, self-hosted file synchronization solution designed to keep folders consistent across multiple macOS devices (such as a MacBook and an iMac) that are not on the same local network. It operates on a client-server model, using a central Django-powered server as a trusted bridge for communication. This architecture provides a private, secure, and highly customizable alternative to commercial cloud storage services, giving users full control over their data and synchronization logic.
The system is composed of two main parts:
A lightweight Python client that runs on each Mac, monitoring a designated folder for changes.
A powerful Django web server deployed on a Linux environment (e.g., Ubuntu), which provides a RESTful API for clients to register, upload, download, and manage file metadata.
By leveraging a centralized server, synMac ensures that file updates, additions, and deletions are reliably propagated to all registered devices, maintaining a single source of truth for the synchronized folder.
