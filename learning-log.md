


# 4 30 21

- if you add multiple transceivers(txc) in the offer/caller, the callee will automatically inherit all those txcs and automatically start receing media for each.

- callees(answer) inherit the callers(offer) transceiver structure
- it is nice how downstreams inherit from upstreams, but it is ultimatly not important, because for robustness and logistical reasons all SFUs in a hierarchy need to know the numvidtracks/numaudtracks by prior agreement rather than just getting this information from upstreams.
- this is so that if a mid-hierarchy SFU restarts it can do its job configuring correctly to downstreams before it gets it's upstream connection.

## we don't actually want downstreams to automatically inherit the full track set available from an SFU
- this is fine for DS (downstream) SFUs, but this is not okay for DS browsers
- we want a downstream browser to only get the first audio and first video track

## Downstream SFUs will know by config the # of audio, video tracks. (YEP)
- this is because we cannot assume a flow of aud/vid track-counts flowing downstream in a hierarchy robust to individual SFU restarts

## The biggest loss of subscriber-offer vs. subscriber-answer is the NULLSFU concept
- in a world where publisher-offers and subscribers-answer a NULLSFU is possible
- in a world where publisher-offers and subscribers-offers a NULLSFU is NOT possible
- this indeed is a loss, but a truly useful NULLSFU would require pub/sub ICE support, 
- which really kind of complicates WHIP and WHEP visions.
- (a one-second delay SDP might work 99% of the time for sort-of-ICE, but experiments are required)

## In a subscriber-offer model, the browser automatically gets single-aud, single-vid WITHOUT fuss 
- In the subscriber-answer model, the browser would need to modify SDP and remove transceivers somehow, which is super messy/complicated for me, and insanely complex for non WebRTC developers

## Conclusions
- SFUs need to know number-video and number-audio beforehand, and can't learn from upstream
- Browsers just need a single audio, single video, and force this easily as the caller
- The WHEP protocol avoids a TXID query-param with the Subscriber-offer-model
- The WHEP protocol avoids double fetching with the Subscriber-offer-model
- The big loss for WHEP using a Subscriber-offer-model is that the NULLSFU is impossible
- It's worth nothing a useful NULLSFU application does require a TURN server, but using a public-IP SFU eliminates the TURN requirement. So applications like www.help.mom under Subscriber-answer-model require TURN, but under Subscriber-offer-model do not require TURN!

## Decision
- WHIEP will use a Subscriber-offer-model, not a Subscriber-answer-model
