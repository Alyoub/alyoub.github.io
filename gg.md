
import Foundation

/// Central Remote Advertising Configuration Model for GoalPulse.
/// Controls Rewarded Ads remotely via GitHub Pages JSON.
public struct AdRemoteConfig: Codable, Equatable {
    public let adsEnabled: Bool
    public let rewardedEnabled: Bool
    public let rewardedEvery: Int
    public let configRefreshIntervalSeconds: TimeInterval

    public init(
        adsEnabled: Bool = true,
        rewardedEnabled: Bool = true,
        rewardedEvery: Int = 6,
        configRefreshIntervalSeconds: TimeInterval = 300
    ) {
        self.adsEnabled = adsEnabled
        self.rewardedEnabled = rewardedEnabled
        self.rewardedEvery = rewardedEvery
        self.configRefreshIntervalSeconds = configRefreshIntervalSeconds
    }

    /// Hard safety minimum for rewardedEvery threshold to prevent invalid or aggressive ad frequencies
    public static let minimumRewardedEvery: Int = 3

    /// Returns whether rewarded ads are active remotely
    public var isRewardedActive: Bool {
        adsEnabled && rewardedEnabled
    }

    /// Returns the effective rewarded interaction threshold (validated, minimum 3)
    public var effectiveRewardedEvery: Int {
        max(rewardedEvery, Self.minimumRewardedEvery)
    }

    /// Hardcoded safe default configuration
    public static var defaultConfig: AdRemoteConfig {
        AdRemoteConfig(
            adsEnabled: true,
            rewardedEnabled: true,
            rewardedEvery: 6,
            configRefreshIntervalSeconds: 300
        )
    }
}
