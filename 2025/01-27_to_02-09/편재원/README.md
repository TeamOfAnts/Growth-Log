# 2025-01-27 ~ 2025-02-09
## 목표
### 회사 코어프로젝트 퀘스트와 비슷하지만 다른 미션 및 상호작용 설계
### 엔터프라이즈 애플리케이션 아키텍쳐 패턴 스터디
---
## 결과
### 
- 상호작용 및 미션 구조 설계(유지보수와 확장성을 고려하여 보상 부분 코틀린의 위임 패턴 활용하여 구현)
```
interface UserChallenge {
    val rewardSource: RewardSource
    fun toChallengeFinishData(reward: UserReward): ChallengeFinishData
    // 각 챌린지별로 보낼 finish 응답 코드를 정의합니다.
    val finishResponseCode: Int
}
```
```
class UserQuest(): UserChallenge {
     var quest: QuestType = getQuest(questId)
        ?: throw IllegalArgumentException("Quest is null. questId: $questId, userId: ${userInfo.userId}")

    override val rewardSource: RewardSource
        get() = quest

    override fun toChallengeFinishData(reward: UserReward): ChallengeFinishData {
        return toQuestFinishData(reward)
    }
    ...
}
```
```
class UserMission() : UserChallenge {
    var mission: MissionType = getMission(missionId)
        ?: throw IllegalArgumentException("Mission is null. missionId: $missionId, userId: ${userInfo.userId}")

    override val rewardSource: RewardSource
        get() = mission

  override fun toChallengeFinishData(reward: UserReward): ChallengeFinishData {
        return toMissionFinishData(reward)
    }
}
```
```
private fun receiveChallengeReward(querySession: QuerySession, userInfo: RPGUserInfo, challenge: UserChallenge) {
    val reward = UserReward(userInfo, challenge.rewardSource)
    userPlayer?.sendPacket(
                challenge.finishResponseCode,
                challenge.toChallengeFinishData(reward)
            )
}
```
- 스터디 후 정리함

### 엔터프라이즈 애플리케이션 아키텍쳐 패턴 스터디
- 2주차(~7장)까지 진행
---
## 교훈
### 회사 신규 미션 구현
- 유지보수에 좋은 아키텍처를 고민하고 현상황에서 어떤 게 최선일지 고민하는 것은 항상 어려운 일임
### 엔터프라이즈 애플리케이션 아키텍처 패턴 스터디
- 역시 완벽히 이해하기엔 아직 실무 경험이 부족한 걸 느낌
---
## 액션 아이템
- 신규 미션의 테스트 및 버그 수정
- 엔터프라이즈 애플리케이션 아키텍처 패턴 스터디 계속 진행