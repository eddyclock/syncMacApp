# syncMacApp
A Private Cross-Network File Synchronization Engine

## Introduction
synMac is a robust, self-hosted file synchronization solution designed to keep folders consistent across multiple macOS devices (such as a MacBook and an iMac) that are not on the same local network. It operates on a client-server model, using a central Django-powered server as a trusted bridge for communication. This architecture provides a private, secure, and highly customizable alternative to commercial cloud storage services, giving users full control over their data and synchronization logic.
The system is composed of two main parts:
A lightweight Python client that runs on each Mac, monitoring a designated folder for changes.
A powerful Django web server deployed on a Linux environment (e.g., Ubuntu), which provides a RESTful API for clients to register, upload, download, and manage file metadata.
By leveraging a centralized server, synMac ensures that file updates, additions, and deletions are reliably propagated to all registered devices, maintaining a single source of truth for the synchronized folder.


## Core Functions & Key Benefits
Here are the primary functions of the synMac system and the significant benefits they provide:

1. Core Function: Secure Client Registration & Authentication
Description: Before a device can participate in the synchronization process, it must be registered with the server. This one-time process generates a unique API Key for the client, which must be included in all subsequent API requests. The server validates this key for every operation, ensuring that only authorized devices can access the file repository.
Benefit: Enhanced Security and Access Control. This function prevents unauthorized access to your files. Unlike simple, unprotected systems, it establishes a trusted relationship between your devices and the server, creating a secure, private sync environment. You have full control over which devices are allowed to connect.

2. Core Function: Centralized State Management with a Database
Description: All file metadata—including filenames, SHA-256 content hashes, modification timestamps, and which client last updated the file—is stored in a robust database (e.g., PostgreSQL) on the server. The server maintains the "authoritative" state of the synchronized folder.
Benefit: Reliability and Data Integrity. Using a database instead of simple file system checks eliminates ambiguity and race conditions. The SHA-256 hash ensures that files are only transferred when their content has actually changed, making the process efficient and preventing unnecessary data transmission. This provides a single source of truth, which is critical for resolving sync states accurately.

3. Core Function: Intelligent Conflict Resolution
Description: The system is designed to handle "conflicts," which occur when the same file is modified on two different devices before either has had a chance to sync. Instead of overwriting one version with another (which would cause data loss), the server identifies the conflict. It saves the conflicting file as a new, clearly named "conflict copy" (e.g., report (conflict from My-MacBook-Air 2025-09-17).docx).
Benefit: Zero Data Loss. This is one of the most critical benefits. It guarantees that work is never silently overwritten. Users are presented with both versions of the file, allowing them to manually review the changes and merge them. This "safety-first" approach is a hallmark of professional-grade synchronization systems like Dropbox or Google Drive.

4. Core Function: Efficient, On-Demand Synchronization Logic
Description: The client doesn't blindly upload or download files. Instead, it first sends a list of its local files and their hashes to the server's /status endpoint. The server compares this with its own authoritative state and responds with a precise list of actions: which files the client needs to upload, which files it needs to download, and which to delete.
Benefit: Optimized Performance and Reduced Network Traffic. This "ask-first" approach is highly efficient. It minimizes data transfer by ensuring only necessary changes are transmitted. For users with limited bandwidth or on metered connections, this significantly reduces network load and speeds up the sync cycle, as unchanged files are simply ignored.

5. Core Function: Soft Deletion and Versioning Potential
Description: When a file is deleted on a client, it is not immediately erased from the server's storage. Instead, it is marked with an is_deleted flag in the database. The physical file can be retained for a period or moved to a "trash" directory.
Benefit: Disaster Recovery and Peace of Mind. Soft deletion provides a crucial safety net against accidental file removal. It allows for the possibility of implementing a "trash" or "restore" feature in the future, where users can recover files that were deleted by mistake. This adds a layer of resilience to the entire system.
