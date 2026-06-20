# Architecture

*A proposed stack for angx-reader.*

---

Individual stewards run the client on any laptop. Spaces run it on a
persistent device — a Raspberry Pi or equivalent.

---

**1. Logs** — angeliaX, built on the Hypercore stack. Hypercore
for append-only signed feeds. Hyperbee for queries by node type,
location, signal type, signal text, and timestamp. Hyperdrive for attachments,
fetched on demand. The reader reads what is already there and changes
nothing.

**2. Transport** — dual-mode. When internet is present, Hyperswarm
handles peer discovery and feed replication. When it is absent, a
Reticulum bridge carries feed updates over whatever medium is available
— LoRa, packet radio, satellite — hop by hop, to the nearest
internet-connected device, which injects the update into the Hypercore
network. When internet returns, Hyperswarm resumes. A bridge between
Hypercore and Reticulum is required here. One master seed derives two
child keys — one for the ANGX feed identity, one for the Reticulum
destination. Twelve words on paper recovers both on a fresh device.

**3. Matching** — `paraphrase-multilingual-MiniLM-L12-v2`, or
equivalent. Multilingual embedding model. Runs on CPU via ONNX 
Runtime. Downloaded once at setup. Converts signal text into a vector
— two signals about the same problem land near each other regardless
of language, with no translation layer. Zero internet, zero cost per
query.

**4. Rules** — a reader script watches `feeds/` for new signals,
passes each to the embedding model, stores the resulting vector in
`vectors/`, and searches for nearby matches. Physical provisions —
food, water, shelter, energy — are distance-filtered against the
node's location field. Informational provisions follow the same rule
when they require presence, and match globally when they do not.
Learning signals match globally. Recent signals rank higher in output.
The underlying record never changes.

**5. Notification** — LXMF, store-and-forward messaging native to
Reticulum, carries match notifications over the same transport as feed
updates. Sideband, an open-source Reticulum client, is where they
arrive.

---

Every component is open source, self-hostable, and replaceable.
No vendor dependency. The right implementation of any layer may
look different from what is proposed here.

---

*angeliaX*
