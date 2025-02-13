    <h1>Nofuture.go - Post-Quantum Cryptographic System</h1>

  
        <h2>MemGuard Initialization & Configuration</h2>
        <pre><code>memguard.CatchInterrupt()
memguard.Purge()
unix.Mlockall(unix.MCL_CURRENT | unix.MCL_FUTURE)</code></pre>
        <ul>
            <li><strong>Secure Memory Locking:</strong> Prevents swapping sensitive data to disk</li>
            <li><strong>Interrupt Handling:</strong> Automatic memory purge on SIGINT/SIGTERM</li>
            <li><strong>Deep Memory Purge:</strong> Secure wiping of allocated buffers</li>
        </ul>


        <h2>MemGuard in Key Lifecycle Management</h2>
        <pre><code>passphrase, _ := memguard.NewImmutableRandom(32)
defer passphrase.Destroy()</code></pre>
        <ul>
            <li><strong>Immutable Buffers:</strong> Write-protected memory regions</li>
            <li><strong>Ephemeral Storage:</strong> Keys exist only in protected memory</li>
            <li><strong>Automatic Destruction:</strong> Guaranteed wipe with defer</li>
        </ul>

        <h2>Enclave-Based Cryptography</h2>
        <pre><code>func deriveEnclaveKey(passphrase *memguard.Enclave) {
    passBuf, _ := passphrase.Open()
    defer passBuf.Destroy()
}</code></pre>
        <ul>
            <li><strong>Double-Layer Protection:</strong> Enclave wrapping + locked buffers</li>
            <li><strong>Controlled Exposure:</strong> Temporary buffer access patterns</li>
            <li><strong>Zero-Copy Architecture:</strong> Minimize memory exposure</li>
        </ul>


        <h2>Quantum-Safe Key Exchange</h2>
        <pre><code>pubKey, secKey, _ := quantumKEMKeyPair()
defer pubKey.Destroy()
defer secKey.Destroy()</code></pre>
        <ul>
            <li><strong>MemGuard-Protected Keys:</strong>
                <ul>
                    <li>Public Key: Immutable locked buffer</li>
                    <li>Private Key: Enclave-wrapped storage</li>
                </ul>
            </li>
            <li><strong>Zeroization on Completion:</strong> Guaranteed key destruction</li>
        </ul>

        <h2>Secure Session Management</h2>
        <pre><code>type QuantumSession struct {
    sessionKey   *memguard.Enclave
    remotePubKey *memguard.Enclave
}</code></pre>
        <ul>
            <li><strong>Enclave-Wrapped Session Keys:</strong> Encrypted memory storage</li>
            <li><strong>Forward Secrecy:</strong> Ephemeral session keys</li>
            <li><strong>Compartmentalization:</strong> Isolated memory regions per session</li>
        </ul>


        <h2>Memory-Hardened Cryptography</h2>
        <pre><code>lockedKey, _ := memguard.NewImmutableFromBytes(key)
defer lockedKey.Destroy()</code></pre>
        <ul>
            <li><strong>Argon2 in Protected Memory:</strong>
                <ul>
                    <li>Memory-hard derivation in locked buffers</li>
                    <li>Secure salt handling</li>
                </ul>
            </li>
            <li><strong>Multi-Layer Protection:</strong>
                <ul>
                    <li>mlock() system calls</li>
                    <li>MADV_DONTDUMP flags</li>
                    <li>Guard pages</li>
                </ul>
            </li>
        </ul>

        <h2>MemGuard Best Practices</h2>
        <ul>
            <li>🔒 Always use <code>defer .Destroy()</code> with LockedBuffers</li>
            <li>🛡️ Prefer <code>NewImmutable</code> for long-lived secrets</li>
            <li>⚠️ Never expose Enclave contents to logging</li>
            <li>🔄 Use rolling buffers for repeated operations</li>
            <li>🧹 Explicit <code>Purge()</code> after critical operations</li>
        </ul>



