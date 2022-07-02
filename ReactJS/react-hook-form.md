# React Hook Form

## Setup

```terminal
npm i react-hook-form
```

## React Hook Form의 장점

- Less code
- Easier Inputs
- Don't deal with events
- Have control over inputs
- Better validation
- Better Errors (set, clear, display)

## register of React Hook Form

> input과 state를 연결시켜준다.  
> 기존에 addEventListener, onChange 로직과 그에 따라 useState값을 변경시키는 로직을 줄여준다.

```typescript
import { useForm formState } from "react-hook-form";

export default function Forms() {
  const { register } = useForm();
  return (
    <form>
      <input {...register("username")} type="text" placeholder="Username" />
      <input type="email" placeholder="Email" />
      <input type="password" placeholder="Password" />
      <input type="submit" value="Submit" />
    </form>
  );
}
```

## validation

```typescript
import { useForm } from "react-hook-form";

interface LoginForm {
  username: string;
}

export default function Forms() {
  const {
    register,
    handleSubmit,
    formState: { errors },
    setError,
  } = useForm<LoginForm>({
    //   mode: "onBlur"
    mode: "onChange",
  });

  const onValid = (data: LoginForm) => {
    console.log("is valid");
  };
  const onInvalid = (errors: FieldErrors) => {
    setError("username", { message: "Not Valid Username" });
  };

  return (
    <form onSubmit={handleSubmit(onValid, onInvalid)}>
      <input
        {...register("username", {
          required: "Username is required",
          minLength: {
            message: "The Usename has to be more than 5 chars",
            value: 5,
          },
        })}
        type="text"
        placeholder="Username"
      />
      {errors.username?.message}
      <input
        {...register("email", {
          required: "Email is required",
          validate: {
            onlyGmail: (value) =>
              value.includes("@gmail.com") || "Only @gmail.com allowed",
          },
        })}
        type="email"
        placeholder="Email"
      />
      {errors.email?.message}
      <input type="submit" value="Submit" />
    </form>
  );
}
```

`handleSubmit`  
event.preventDefault()와 같은 역할을 한다.  
handleSubmit 함수 호출 후 리턴되는 함수가 onSubmit 이벤트에 호출될 함수가 된다.  
인자로 받는 onValid와 onInValid로 validation 검사를 할 수 있어, 기존 방식처럼 if문으로 validation을 검사하지 않아도 된다.

`validation을 위한 register options`  
register 함수의 type을 살펴보면 validation property에는 `required` 외에도 `min, max, maxLength, minLength, pattern` 등이 있다는 것을 확인할 수 있다.

`onBlur`  
mode를 onBlur로 설정한 경우 input의 바깥에 focus가 되었을 때 validation이 발생한다.

`onChange`  
mode를 onChange로 설정할 경우 input이 변화할 때마다 validation이 발생한다.

### `결론`

!! react-hook-form에서 validation을 사용할 때는 validation이 바로 에러에 연결된다.

[공식 문서](https://react-hook-form.com/) |
[참고 강의](https://nomadcoders.co/carrot-market/lectures/3532)
