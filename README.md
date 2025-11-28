# UE5-movement// MyCharacter.cpp

#include "MyCharacter.h"
#include "GameFramework/CharacterMovementComponent.h"
#include "GameFramework/PlayerController.h"
#include "EnhancedInputSubsystems.h"
#include "EnhancedInputComponent.h"

AMyCharacter::AMyCharacter()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyCharacter::BeginPlay()
{
    Super::BeginPlay();
    
    // Дополнительная настройка: например, включение ориентации движения к направлению взгляда
    // GetCharacterMovement()->bOrientRotationToMovement = true; 
}

void AMyCharacter::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}

// Привязка ввода (для старой системы ввода Axis Mapping)
void AMyCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
    Super::SetupPlayerInputComponent(PlayerInputComponent);

    // Пример привязки осей (если вы используете устаревшую систему ввода)
    PlayerInputComponent->BindAxis(TEXT("MoveForward"), this, &AMyCharacter::MoveForward);
    PlayerInputComponent->BindAxis(TEXT("MoveRight"), this, &AMyCharacter::MoveRight);
    // Примечание: в UE5 рекомендуется использовать новую систему Enhanced Input
}

void AMyCharacter::MoveForward(float AxisValue)
{
    // Добавляет вход движения вдоль вектора вперед персонажа
    if ((Controller != nullptr) && (AxisValue != 0.0f))
    {
        // Найти направление вперед
        const FVector Direction = FRotationMatrix(Controller->GetControlRotation()).GetScaledAxis(EAxis::X);
        AddMovementInput(Direction, AxisValue);
    }
}

void AMyCharacter::MoveRight(float AxisValue)
{
    // Добавляет вход движения вдоль вектора вправо персонажа
    if ((Controller != nullptr) && (AxisValue != 0.0f))
    {
        // Найти направление вправо
        const FVector Direction = FRotationMatrix(Controller->GetControlRotation()).GetScaledAxis(EAxis::Y);
        AddMovementInput(Direction, AxisValue);
    }
}
