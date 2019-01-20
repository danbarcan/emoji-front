package com.emojichallenges.game.controllers;

import com.emojichallenges.game.entities.Challenge;
import com.emojichallenges.game.payloads.ApiResponse;
import com.emojichallenges.game.payloads.ChallengePayload;
import com.emojichallenges.game.repositories.ChallengeRepository;
import com.emojichallenges.game.repositories.EmojiRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import java.util.List;
import java.util.Optional;

@RestController
public class ChallengeController {
    @Autowired
    EmojiRepository emojiRepository;

    @Autowired
    ChallengeRepository challengeRepository;

    @GetMapping("/challenge")
    public ResponseEntity<Challenge> getChallenge(@RequestParam Long id) {
        Optional<Challenge> challenge = challengeRepository.findById(id);

        return ResponseEntity.of(challenge);
    }

    @GetMapping("/challenges")
    public ResponseEntity<List<Challenge>> getChallenge() {
        List<Challenge> challenges = challengeRepository.findAll();

        return ResponseEntity.ok(challenges);
    }

    @PostMapping("/challenge")
    public ResponseEntity<ApiResponse> saveChallenge(@Valid @RequestBody ChallengePayload challengePayload) {
        if (challengeRepository.existsByName(challengePayload.getName())) {
            return new ResponseEntity<>(new ApiResponse(false, "Challenge name already exists!"),
                    HttpStatus.BAD_REQUEST);
        }

        challengeRepository.save(Challenge.builder()
                .name(challengePayload.getName())
                .emojis(emojiRepository.findAllByIdIn(challengePayload.getEmojis()))
                .solution(challengePayload.getSolution())
                .build());

        return ResponseEntity.ok(new ApiResponse(true, "Challenge saved successfully"));
    }
}
