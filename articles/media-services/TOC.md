# [Übersicht](media-services-overview.md)
## [Konzepte](media-services-concepts.md)


# Erste Schritte
## [Erstellen und Verwalten eines Kontos](media-services-portal-create-account.md)
## [Einrichten Ihrer Entwicklungsumgebung](media-services-set-up-computer.md)
## Übertragen von Video on Demand
### [Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Ausführen von Livestreaming
### [Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Anleitung
## Verwalten
### [Verwalten von Streamingendpunkten im Portal](media-services-portal-manage-streaming-endpoints.md)
### Verwalten von Entitäten
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Verwalten von Konten mit PowerShell](media-services-manage-with-powershell.md)
### [Zuschneiden von Videos mit Media Encoder Standard](media-services-crop-video.md)
### [Vorgehensweise: Aktualisieren von Media Services nach dem Austausch der Speicherzugriffsschlüssel](media-services-roll-storage-access-keys.md)
### [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)
### Filter
#### [Erstellen von Filtern mit dem Azure Media Services .NET SDK](media-services-dotnet-dynamic-manifest.md)
#### [Codieren eines Medienobjekts mit Media Encoder Standard](media-services-rest-encode-asset.md)
### Programmgesteuerter Verbindung
#### [.NET](media-services-dotnet-connect-programmatically.md)
#### [REST](media-services-rest-connect-programmatically.md)

## Hochladen von Inhalten
### Hochladen von Dateien in ein Konto
#### [Portal](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Kopieren vorhandener Blobs](media-services-copying-existing-blob.md)

## Codieren
### [Inhalt](media-services-encode-asset.md)
#### Codieren eines Medienobjekts mit Media Encoder Standard
##### [Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
#### [Generieren von Miniaturansichten mithilfe von Media Encoder Standard mit .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
#### [Erweiterte Codierung](media-services-advanced-encoding-with-mes.md)
##### [Media Encoder Premium-Workflow](media-services-encode-with-premium-workflow.md)
##### [Tutorials für den Media Encoder Premium-Workflow](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Erstellen von erweiterten Codierungsworkflows mit Workflow-Designer](media-services-workflow-designer.md)
##### [Premium-Workflow mit mehreren Eingaben](media-services-media-encoder-premium-workflow-multiplefilesinput.md)

#### Schemas 
#####[Media Encoder Standard](media-services-mes-schema.md)
#####[Eingeben von Metadaten](media-services-input-metadata-schema.md)
#####[Ausgeben von Metadaten](media-services-output-metadata-schema.md)

#### Ältere Encoder
##### [Verwenden von Azure Media Packager](media-services-static-packaging.md)

### [Livestreams](media-services-manage-channels-overview.md)
#### [Lokale Encoder](media-services-live-streaming-with-onprem-encoders.md)
#### Tutorials für lokale Encoder
##### [Portal](media-services-portal-live-passthrough-get-started.md)
##### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
#### [Livestreaming mit Cloudencoder](media-services-manage-live-encoder-enabled-channels.md)
#### Tutorials für Cloudencoder
##### [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
##### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
#### [Konfigurieren von lokalen Encodern für die Verwendung mit einem Cloudencoder](media-services-live-encoders-overview.md)
#### [Behandeln von Vorgängen mit langer Ausführungsdauer](media-services-dotnet-long-operations.md)
#### [Spezifikation für die Liveerfassung von fragmentiertem MP4](media-services-fmp4-live-ingest-overview.md)
#### [Dynamische Paketerstellung](media-services-dynamic-packaging-overview.md)

### Medienverarbeitung
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)

### Konfigurieren von Encodern für einen Single-Bitrate-Livestream
#### [Elemental Live-Encoder](media-services-configure-elemental-live-encoder.md)
#### [FMLE-Encoder](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster-Encoder](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast-Encoder](media-services-configure-wirecast-live-encoder.md)

## [Schützen](media-services-content-protection-overview.md)
### [Konfigurieren von Content Protection im Portal](media-services-portal-protect-content.md)
### [Konfigurieren eines unverschlüsselten AES-128-Schlüssels für Ihren Datenstrom](media-services-protect-with-aes128.md)
### [Verschlüsseln von Inhalten mit der Speicherverschlüsselung und AMS-REST-API](media-services-rest-storage-encryption.md)
### [Media Services PlayReady-Lizenzvorlage – Übersicht](media-services-playready-license-template-overview.md)
### [DRM-Lizenzbereitstellung](media-services-deliver-keys-and-licenses.md)
### [Übermitteln von Widevine-Lizenzen an Azure Media Services mithilfe von Partnern](media-services-licenses-partner-integration.md)
### [Verwenden von dynamischer allgemeiner Verschlüsselung mit PlayReady und/oder Widevine.](media-services-protect-with-drm.md)
### [Verwenden von Azure Media Services zum Streamen von durch Apple FairPlay geschützten HLS-Inhalten](media-services-protect-hls-with-fairplay.md)
### [CENC mit mehreren DRM-Systemen und Access Control: Referenzentwurf und -implementierung in Azure und Azure Media Services](media-services-cenc-with-multidrm-access-control.md)

### Medienobjektübermittlung
#### Konfigurieren von Übermittlungsrichtlinien für Medienobjekte
##### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
##### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Erstellen von ContentKeys
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel
#### [Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Analysieren](media-services-analytics-overview.md)
### [Verarbeiten mit Indexer 2](media-services-process-content-with-indexer2.md)
### [Verarbeiten mit Indexer](media-services-index-content.md)
### [Verarbeiten mit Hyperlapse](media-services-hyperlapse-content.md)
### [Verarbeiten mit Gesichtserkennung](media-services-face-and-emotion-detection.md)
### [Verarbeiten mit Bewegungserkennung](media-services-motion-detection.md)
### [Verarbeiten mit Gesichtsbearbeitung](media-services-face-redaction.md)
### [Verarbeiten mit Videominiaturansichten](media-services-video-summarization.md)
### [Verarbeiten mit OCR](media-services-video-optical-character-recognition.md)

## Skalieren
### [Medienverarbeitung](media-services-scale-media-processing-overview.md)
#### [Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
#### [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
### Streamingendpunkte
#### [Portal](media-services-portal-scale-streaming-endpoints.md)

## [Bereitstellen von Inhalten](media-services-deliver-content-overview.md)
### [Übersicht über Filter und dynamische Manifeste](media-services-dynamic-manifest-overview.md)
### Erstellen von Filtern
#### [.NET](media-services-dotnet-dynamic-manifest.md)
#### [REST](media-services-rest-dynamic-manifest.md)
### Veröffentlichen von Inhalten
#### [Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [Bereitstellen per Download](media-services-deliver-asset-download.md)
### [Failoverstreamingszenario](media-services-implement-failover.md)

## Nutzen
### [Wiedergeben von Medien mit vorhandenen Playern](media-services-playback-content-with-existing-players.md)
### [Wiedergeben von Medien mit Media Player](media-services-develop-video-players.md)
### Weitere Wiedergabeoptionen
#### [Windows Store-Anwendung mit Smooth Streaming](media-services-build-smooth-streaming-apps.md)
#### [HTML5-Anwendung mit DASH.js](media-services-embed-mpeg-dash-in-html5.md)
#### [Adobe Open Source Media Framework-Player](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [Einfügen von Anzeigen auf Clientseite](media-services-inserting-ads-on-client-side.md)

## Integrieren
### [CDN-Cachingrichtlinie in der Media Services-Erweiterung](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### [Lizenzieren des Portierungskits für den Microsoft†" Smooth Streaming-Client](media-services-sspk.md)
### [Speicherkontenübergreifendes Verwalten von Assets](meda-services-managing-multiple-storage-accounts.md)
### [Bereitstellen von Widevine-Lizenzen für Azure Media Services mithilfe von Axinom](media-services-axinom-integration.md)
### [Übermitteln von Widevine-Lizenzen an Azure Media Services mithilfe von castLabs](media-services-castlabs-integration.md)
### [Übersicht über die Widevine-Lizenzvorlage](media-services-widevine-license-template-overview.md)

## Überwachen
### Prüfen des Auftragsstatus
#### [REST](media-services-rest-check-job-progress.md)
#### [Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Queue Storage zum Überwachen von Auftragsbenachrichtigungen](media-services-dotnet-check-job-progress-with-queues.md)

## Problembehandlung
### [Häufig gestellte Fragen](media-services-frequently-asked-questions.md)
### [Anleitung zur Behandlung von Problemen bei Livestreaming](media-services-troubleshooting-live-streaming.md)
###[Fehlercodes](media-services-error-codes.md)
###[Wiederholungslogik](media-services-retry-logic-in-dotnet-sdk.md)

# Referenz
## [Versionshinweise](media-services-release-notes.md)
## [.NET](media-services-dotnet-how-to-use.md)
## [REST](media-services-rest-how-to-use.md)
## [Media Encoder Premium Workflow-Formate und -Codecs](media-services-premium-workflow-encoder-formats.md)
## [Media Encoder Standard-Formate und -Codecs](media-services-media-encoder-standard-formats.md)

# Ressourcen
## [Preise](https://azure.microsoft.com/pricing/details/media-services/)
## [Azure Media Services-Community](media-services-community.md)











<!--HONumber=Nov16_HO2-->


